# Python爬取动态资源

基本思路是依靠**正则表达式**搜索页面源代码中对相关文件的引用，css及js文件使用**unicode型的文本数据**保存，图片文件使用**bytes型的二进制数据**保存。

## 规则匹配

### **css文件**

1. **'<link href="(.*?)"'**
2. **'<link rel="stylesheet" href="(.*?)"'**
3. **'<link type="text/css" rel="stylesheet" href="(.*?)"'**

### **js文件**

1. **'<script src="(.*?)"'**
2. **'<script type="text/javascript" src="(.*?)"'**

### **图片文件**

1. **'<img src="(.*?)"'**
2. **'<img.\*？src="(.*?)"'**

**部分css和js文件是以http链接形式引用的，匹配后的字符串应优先判断是否为http引用**

**保存图片时应先判明其后缀名**

## request.get常用参数

- **url**：请求的地址
- **data**：请求发送的数据
- **heades**：请求头，包含浏览器信息以及cookie等，类型为字典，数据可由浏览器抓包获得，通常情况下需包含浏览器信息以通过较宽松的人机验证，cookie可用于身份验证
- **timeout**：设置超时时间，响应超过该时间便会判定为超时
- **proxy**：代理服务器

## 代码示例

~~~python
import re
import requests
import os

def loadheaders(file):	#从文件"file"中载入请求头信息字典，"file"为字典文件路径
    file=open(file,"r+")
    str=""
    for line in file:
        str+=line
    str = re.sub(r"\n", "','", str)
    str = re.sub(r": ", "': '", str)
    str="{'"+str+"'}"
    headers=eval(str)
    file.close()
    return headers

def get_html(url):	#请求页面
    headers=loadheaders("./resource/headers.headers")
    response = requests.get(url, timeout=10, headers=headers)	#设置超时判定
    response.encoding = 'utf8'
    html = response.text	#unicode型文本数据
    return html

def get_pic(url):	#请求图片
    headers=loadheaders("./resource/headers.headers")
    pic_response = requests.get(url, timeout=10, headers=headers)
    pic = pic_response.content	#bytes型二进制数据
    return pic


def save_file(chdir_path, filename, content):	#保存文件
    if filename[-4:] in ['.jpg', '.png', 'webp','.png','jpeg', '.gif', '.bmp']:	#识别保存并保存
        with open(chdir_path + '/' + 'images/' + filename , "wb+") as f:
            f.write(content)
            return print('写入{}'.format(filename) + '成功')
    elif filename[-2:] == 'js':	#识别js并保存
        with open(chdir_path + '/' + 'js/' + filename, 'w+',encoding='utf-8') as f:
            f.write(content)
            return print('写入{}'.format(filename)+'成功')
    elif filename[-3:] == 'css':	#识别css并保存
        with open(chdir_path + '/' + 'css/' + filename, 'w+',encoding='utf-8') as f:
            f.write(content)
            return print('写入{}'.format(filename)+'成功')
    else:
        with open(chdir_path + '/' + filename, 'w+',encoding='utf-8') as f:
            f.write(content)
            return print('写入{}'.format(filename) + '成功')

def scarpy_web(url, web_name):
    local_path = os.getcwd()	#返回当前工作目录
    if not os.path.exists(web_name):
        os.makedirs(web_name+'/images')
        os.makedirs(web_name+'/css')
        os.makedirs(web_name+'/js')	#创建分支目录

    content = get_html(url)
    filename = web_name + '.html'
    chdir_path = local_path + '/' + web_name	#保存文件的名称

    save_file(chdir_path, filename, content)	#保存html

    patterncss1 = '<link href="(.*?)"'
    patterncss2 = '<link rel="stylesheet" href="(.*?)"'
    patterncss3 = '<link type="text/css" rel="stylesheet" href="(.*?)"'	#设置css文件匹配规则
    result = re.compile(patterncss1, re.S).findall(content)
    result += re.compile(patterncss2, re.S).findall(content)
    result += re.compile(patterncss3, re.S).findall(content)	#从页面源码中查找css引用并合并结果

    for link in result:
        css_name_model = '.*/(.*?).css'
        css_filename = re.compile(css_name_model, re.S).findall(link)#过滤结果
        try:
        # 这里匹配出来有些css和js文件是以http链接形式引用的，所以分开来写入
            if link[0] == 'h':	#识别由http链接引用的css
                resopnse1 = get_html(link)	#请求css文件
                css_name1 = css_filename[0]+'.css'
                save_file(chdir_path, css_name1, resopnse1)
            else:	#保存其余css文件
                css_url = url+link
                resopnse2 = get_html(css_url)
                css_name2 = css_filename[0]+'.css'
                save_file(chdir_path, css_name2, resopnse2)
        except Exception as e:
            print('css{}匹配失败'.format(css_filename), '原因:', e)

    patternjs1 = '<script src="(.*?)"'
    patternjs2 = '<script type="text/javascript" src="(.*?)"'	#设置js文件匹配规则
    list = re.compile(patternjs1, re.S).findall(content)
    list += re.compile(patternjs2, re.S).findall(content)

    for i in list:
        js_name_model = '.*/(.*?).js'
        js_filename = re.compile(js_name_model, re.S).findall(i)
        try:
            if i[0] == 'h':
                html1 = get_html(i)
                js_filename1 = js_filename[0]+'.js'
                save_file(chdir_path, js_filename1, html1)
            else:
                js_url = url+i
                html2 = get_html(js_url)
                js_filename2 = js_filename[0]+'.js'
                save_file(chdir_path, js_filename2, html2)
        except Exception as e:
            print('js{}匹配失败'.format(js_filename), '原因:', e)

    patternimg = '<img src="(.*?)"'
    patternimg2 = '<img.*?src="(.*?)"'	#设置图片文件匹配规则
    pic_list = re.compile(patternimg, re.S).findall(content)
    pic_list += re.compile(patternimg2, re.S).findall(content)

    #print(pic_list)
    for i in pic_list:
        pic_name_model = '.*/(.*?).(jpg|webp|png|jpeg|gif|bmp)'
        pic_filename = re.compile(pic_name_model, re.S).findall(i)
        try:
            if i[0] == 'h':
                pic1 = get_pic(i)
                pic_filename1 = pic_filename[0][0]+'.' + pic_filename[0][1]	#获取图片后缀名
                save_file(chdir_path, pic_filename1, pic1)
            else:
                pic2 = url+i
                pic2 = get_pic(pic2)
                pic_filename2 = pic_filename[0][0]+'.' + pic_filename[0][1]
                save_file(chdir_path, pic_filename2, pic2)
        except Exception as e:
            print('图片{}匹配识别'.format(pic_filename), '原因', e)

if __name__ == '__main__':
    # url = input('输入你想要爬取的网页:')
    url = 'https://www.baidu.com/?tn=44004473_39_oem_dg'
    rurl=url.split('/')
    web_name = rurl[2]
    scarpy_web(url=url, web_name=web_name)
~~~
