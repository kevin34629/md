# 介绍

> 用于和mirai的拓展，用于爬取pixiv上的图片并发送

命令格式

~~~
/pid <pid>
~~~

> 通过pid搜索对应网页中的原图并下载，然后返回File对象

# 建立连接

> 命令**"/pid 96261214"**，其有效pid为96261214

~~~java
String[] s=command.split(" ");
String pid=s[1];	//获取pid
String url0="https://www.pixiv.net/artworks/"+pid;	//拼接链接
~~~

> 建立连接，其中String向URL转换时会抛出**java.net.MalformedURLException**异常需使用catch捕获

~~~java
try{
	URL url = new URL(url0);
	URLConnection 	       
	urlConnection=url.openConnection();	//建立连接
	urlConnection.addRequestProperty("User-Agent", "Mozilla");	//请求头信息
}catch (Exception ignore){}
~~~

> 其中**openConnection()**方法会返回状态码，可获取状态码进行后续操作

# 获取原图URL

> 通过**正则表达式匹配原图的URL**，并将其添加到ArrrayList<URL>中，通过观察，得到正则表达式如下

~~~java
String pattern="https://i.pximg.net/img-original/.*?(png|jpg)";
~~~

> 其中**“.*”**为匹配任意字符多次，其后**"?"**表示勉强匹配，避免匹配到URL外的部分

~~~java
reader=new BufferedReader(new InputStreamReader(urlConnection.getInputStream(),"GBK"));	
//建立输入流，编码为GBK
while((line=reader.readLine())!=null){
	Matcher m= Pattern.compile(pattern).matcher(line);	//进行匹配
	while(m.find()){
		URL rurl=new URL(m.group());
		result.add(rurl);	//将结果添加至ArrrayList<URL>中
	}
}
~~~

# 下载图片

~~~java
try
{
	SslUtils.ignoreSsl();
	URLConnection con = url.openConnection();
	InputStream is = con.getInputStream();	//创建输入流
	byte[] bs = new byte[1024];	//1Kb缓冲区
	int len;	//读取到的数据长度
	FileOutputStream os = new FileOutputStream(file, true);	//创建输出流，boolean append=true（追加）
	while ((len = is.read(bs)) != -1) {
		os.write(bs, 0, len);	//写入数据，偏移量为0
	}
	os.close();
	is.close();
}
catch (Exception e){System.out.println(e);}
~~~

> 其中FileOutputStream会抛出java.io.IOException异常，需捕获处理

# SSL处理

> 启用https连接需要证书，此处处理方法为忽略SSL验证

~~~java
class SslUtils {
    public static void trustAllHttpsCertificates() throws Exception {
        TrustManager[] trustAllCerts = new TrustManager[1];
        TrustManager tm = new miTM();
        trustAllCerts[0] = tm;
        SSLContext sc = SSLContext.getInstance("SSL");
        sc.init(null, trustAllCerts, null);
        HttpsURLConnection.setDefaultSSLSocketFactory(sc.getSocketFactory());
    }
    static class miTM implements TrustManager, X509TrustManager {
        public X509Certificate[] getAcceptedIssuers() {
            return null;
        }
        public void checkServerTrusted(X509Certificate[] certs, String authType) {}
        public void checkClientTrusted(X509Certificate[] certs, String authType) {}
    }
    public static void ignoreSsl() throws Exception{
        HostnameVerifier hv = (urlHostName, session) -> true;
        trustAllHttpsCertificates();
        HttpsURLConnection.setDefaultHostnameVerifier(hv);
    }
}
~~~

> 在建立SSL连接之前调用本类的**ignoreSsl()**函数即可

# 遗留问题

本次连接pixiv使用了nginx建立本地反向代理，在访问原图时会出现403错误，为代理配置的问题