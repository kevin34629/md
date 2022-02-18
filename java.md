# 语法

## StringBuffer 和 StringBuilder 类

### **介绍**

和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。

StringBuilder 和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的（不能同步访问）。

由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。

### **StringBuffer方法**

* **`StringBuffer`** appendCodePoint(**`int`** codePoint) : <font color="#2383CE">将 codePoint 参数的字符串表示形式追加到此序列</font>
* **`int`** capacity() : <font color="#2383CE">返回当前容量</font>
* **`StringBuffer`** reverse() : <font color="#2383CE">返回此字符序列的反向替换</font>
* **`void`** trimToSize() : <font color="#2383CE">减少用于字符序列的存储空间至当前大小</font>
* **`CharSequence`** subSequence(**`int`** start, **`int`** end) : <font color="#2383CE">返回一个新的字符序列，它是该序列的子序列</font>
* **`String`** substring(**`int`** start, **`int`** end) : <font color="#2383CE">返回一个新的 String ，其中包含当前包含在此序列中的字符的子序列</font>，<font color="#FFA561">第二个参数可选</font>

### **StringBuilder方法**

* **`StringBuffer`** appendCodePoint(**`int`** codePoint) : <font color="#2383CE">将 codePoint 参数的字符串表示形式追加到此序列</font>
* **`int`** capacity() : <font color="#2383CE">返回当前容量</font>
* **`StringBulider`** reverse() : <font color="#2383CE">返回此字符序列的反向替换</font>
* **`void`** trimToSize() : <font color="#2383CE">减少用于字符序列的存储空间至当前大小</font>

## Lambda表达式

### **语法格式**

~~~java
(parameters) ->{ statements; }
~~~

### **简例**

~~~java
final static String salutation = "Hello! ";
public static void main(String args[]){
GreetingService greetService1 = message -> 
System.out.println(salutation + message);
greetService1.sayMessage("World");
}
interface GreetingService {
	void sayMessage(String message);
}
~~~

### **注意事项**

* lambda表达式只能引用标记了final的外层局部变量，这就是说不能在lambda内部修改定义在域外的局部变量
* lambda表达式的局部变量可以不用声明为final，但是必须不可被后面的代码修改（隐式final）

## Data类

**方法**

* **`long`** getTime() : <font color="#2383CE">返回时间戳（自1970年1月1日00:00:00GMT以来的毫秒数）</font>
* **`void`** setTime(**`long`** time) : <font color="#2383CE">用时间戳设置时间和日期</font>
* **`String`** toString() :  <font color="#2383CE">把此 Date 对象转换为以下形式的 String：dow mon dd hh:mm:ss zzz yyyy</font>，<font color="#FFA561">dow 是一周中的某一天 (Sun, Mon, Tue, Wed, Thu, Fri, Sat)</font>

## 正则表达式

<font color="#F00001">在 Java 中，**`\\`** 表示：**我要插入一个正则表达式的反斜线，所以其后的字符具有特殊的意义。**</font>

### **索引方法**

* **`int`** start() : <font color="#2383CE">返回以前匹配的初始索引</font><font color="#FFA561">（匹配项第一个字符的位置）</font>
* **`int`** start(int group) : <font color="#2383CE">返回在以前的匹配操作期间，由给定组所捕获的子序列的初始索引</font>
* **`int`** end() : <font color="#2383CE">返回最后匹配字符之后的偏移量</font><font color="#FFA561">（匹配项最后一个字符位置+1）</font>
* **`int`** end(**`int`** group) : <font color="#2383CE">返回在以前的匹配操作期间，由给定组所捕获子序列的最后字符之后的偏移量</font>

### **查找方法**

* **`boolean`** lookingAt() : <font color="#2383CE">尝试将从区域开头开始的输入序列与该模式匹配</font>
* **`boolean`** find() : <font color="#2383CE">尝试查找与该模式匹配的输入序列的下一个子序列</font>
* **`boolean`** find(**`int`** start) : <font color="#2383CE">重置此匹配器，然后尝试查找匹配该模式、从指定索引开始的输入序列的下一个子序列</font>
* **`boolean`** matches() : <font color="#2383CE">尝试将整个区域与模式匹配</font>

### **替换方法**

* **`Matcher`** appendReplacement(**`StringBuffer`** sb, **`String`** replacement) : <font color="#2383CE">实现非终端添加和替换步骤</font>

* **`StringBuffer`** appendTail(**`StringBuffer`** sb) : <font color="#2383CE">实现终端添加和替换步骤</font>

  ***替换示例***

  ~~~java
  while(m.find){
      m.appendReplacement(sb,replaceContext);
  }
  m.appendTail(sb);
  ~~~

* **`String`** replaceAll(**`String`** replacement) : <font color="#2383CE">替换模式与给定替换字符串相匹配的输入序列的每个子序列</font>

* **`String`** replaceFirst(**`String`** replacement) : <font color="#2383CE">替换模式与给定替换字符串匹配的输入序列的第一个子序列</font>

* **`String`** quoteReplacement(**`String`** s) : <font color="#2383CE">返回指定字符串的字面替换字符串</font>

## 控制台读取

~~~java
	BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	String str;
	System.out.println("Enter lines of text.");
	System.out.println("Enter 'end' to quit.");
	do {
	str = br.readLine();
	System.out.println(str);
	} while (!str.equals("end"));	//输入"end"结束读取
~~~

## 逐行读取

~~~java
File file = new File("D:/34629/Desktop/Java/untitled2/test.txt");
List<String> ss = Files.readAllLines(file.toPath());
for(String s:ss){System.out.println(s);}
~~~

## 逐行写入

~~~java
FileOutputStream output = new FileOutputStream(file);
	OutputStreamWriter writer = new OutputStreamWriter(output, "UTF-8");
	for(String s:ss)
	{
		if(Objects.equals(s,"@！#！#！@3")){continue;}	//跳过"@！#！#！@3"这一行的写入
		writer.append(s);
		writer.append("\r\n");
	}
	writer.close();
	output.close();
~~~

# 常见数据结构

## Bitset（位图）

### **作用**

一个Bitset类创建一种特殊类型的数组来保存位值。BitSet中数组大小会随需要增加。

### **例场景**

目前很多应用都把手机号码作为登录的账户名，此处介绍一种高效的基于手机号，来实现黑白名单的方案。

假设有一个0到31的集合，集合里面的元素不重复，比如这样{0,3,1,5,2,19,7,8,31,21,10}。通过位图，可以将这样的集合表示为11110001101000000001010000000001, 其中1表示该数值为下标的数存在在集合中，比如第一个1表示0存在集合中，第二个1表示1存在集合中，等等。

通过这样做，起码可以得到两个好处

1. 节省空间–我们可以用二进制一个位来存储存储两个信息，一是存不存在，而是存在的数是多少.

2. 排序–从左到右遍历这个为图，可以得到排序的集合，比如上例中，可以得到集合 {0,1,2,3,7,8,10,19,21,31}
   如果将所有这电话号码用位图表示，那么需要9999999999个bit（10个9，考虑到手机号码的第一位都是1）。9999999999 bit =（9999999999/8）byte = (9999999999 / (8 * 1024)) KB = (9999999999 / 8 10241024) M = 1192M

1192M，内存占有还是太大，我们前期的黑白名单所占内存远远低于这个值。 考虑到手机号码的前3位都差不多，而且总数最多30个，所以考虑到手机号码拆分为两部分(头3位和剩余8位)，这样就把位图的位数就降2个数量级了。所以如果要在内存中装入手机号码后8位，需要99999999（8个9）个bit，算一下：99999999 bit =（99999999/8）byte = (99999999 / (8 * 1024)) KB = (99999999 / 8 10241024) M = 11.92M。

内存中装入头2位（手机号第一位始终为1，因此只需装入后面2位），需要99（2个9）个bit，算下
99 bit = 99/8 byte =12.375 byte。

大约12M 内存，就能表示所有手机号码，达到黑白名单。

### **方法**

* **`void`** and(**`BitSet`** set)

  **简例**

  ~~~java
  BitSet set0 = new BitSet();
  set0.set(2);
  set0.set(4);
  BitSet set1 = new BitSet();
  set0.and(set1);
  set0.set(3);
  System.out.println(set0);
  ~~~

  ***结果***

  ~~~java
  {3}
  ~~~

* **`void`** andNot(**`BitSet`** set) : <font color="#2383CE">清除此BitSet在参数BitSet中对应的所有的位</font>

  **简例**

  ~~~java
  BitSet set0 = new BitSet();
  set0.set(2);
  set0.set(4);
  BitSet set1 = new BitSet();
  set1.set(2);
  set0.set(3);
  set0.andNot(set1);
  System.out.println(set0);
  ~~~

  ***结果***

  ~~~java
  {3, 4}
  ~~~

* **`int`** cardinality() : <font color="#2383CE">返回此BitSet中设置为true的位数</font>

* **`void`** clear(**`int`** startIndex, **`int`** endIndex) : <font color="#2383CE">将指定的startIndex（**包括**）到指定的toIndex（**不包括**）范围内的位设置为 false</font>

* **`void`** flip(**`int`** index) : <font color="#2383CE">将指定索引处的位设置为其当前值的补码</font>

* **`void`** flip(**`int`** startIndex, **`int`** endIndex) : <font color="#2383CE">将指定的fromIndex（**包括**）到指定的toIndex（**不包括**）范围内的每个位设置为其当前值的补码</font>

* **`int`** hashCode() : <font color="#2383CE">返回此Bitset的哈希码值</font>

* **`boolean`** intersects(**`BitSet`** bitSet) : <font color="#2383CE">如果指定的BitSet中有设置为true的位，并且在此BitSet中也将其设置为true，则返回true</font>

* **`int`** length() : <font color="#2383CE">返回此BitSet的"逻辑大小"：BitSet 中最高设置位的索引加1</font>

* **`void`** set(**`int`** index, **`boolean`** v) : <font color="#FFA561">第二个参数可选，默认为true</font>

* **`void`** set(**`int`** startIndex, **`int`** endIndex, **`boolean`** v) : <font color="#FFA561">第三个参数可选，默认为true</font>

* **`int`** size() : <font color="#2383CE">返回此BitSet表示位值时实际使用空间的位数</font>

## Vector（向量）

### **作用**

Vector类实现了一个动态数组。和ArrayList很相似，但**Vector中的操作是线程安全的**。

### **构造**

Vector 类支持 4 种构造方法。

1. 创建一个默认的向量，默认大小为<font color="#F00001">**10**</font>：

```java
Vector()
```

2. 构造方法创建指定大小的向量

```java
Vector(int size)
```

3. 创建指定大小的向量，并且增量用incr指定。增量表示向量每次增加的元素数目。

```java
Vector(int size,int incr)
```

4. 创建一个包含集合c元素的向量：

```java
Vector(Collection c)
```

### **方法**

* **`void`** addElement(**`Object`** obj) : <font color="#2383CE">将指定的组件添加到此向量的末尾，将其大小增加1</font>
* **`int`** capacity() :  <font color="#2383CE">返回此向量的当前容量</font>
* **`void`** copyInto(**`Object[]`** anArray) : <font color="#2383CE">将此向量的组件复制到指定的数组中</font>
* **`Object`** elementAt(**`int`** index) : <font color="#2383CE">返回指定索引处的组件</font>
* **`void`** ensureCapacity(**`int`** minCapacity) : <font color="#2383CE">增加此向量的容量（如有必要），以确保其至少能够保存最小容量参数指定的组件数</font>
* **`int`** indexOf(**`Object`** elem, **int** index) : <font color="#FFA561">第二个参数可选，默认为0，如果此向量不包含该元素，则返回 -1</font>
* **`int`** lastIndexOf(**`Object`** elem, **`int`** index) : <font color="#FFA561">第二个参数可选，默认为向量容量大小，如果此向量不包含该元素，则返回 -1</font>
* **`void`** removeAllElements() : <font color="#2383CE">从此向量中移除全部组件，并将其大小设置为零</font>
* **`boolean`** removeElement(**`Object`** obj) :  <font color="#2383CE">从此向量中移除变量的第一个（索引最小的）匹配项</font>
* **`boolean`** retainAll(**`Collection`** c) : <font color="#2383CE">在此向量中仅保留包含在指定 Collection 中的元素</font>
* **`Object`** set(**`int`** index, **`Object`** element) : <font color="#2383CE">用指定的元素替换此向量中指定位置处的元素，返回值为被替换的元素</font>
* **`void`** setElementAt(**`Object`** obj, **`int`** index) : <font color="#2383CE">将此向量指定index处的组件设置为指定的对象</font>
* **`Object[]`** toArray(**`Type[]`** a) : <font color="#2383CE">返回一个包含向量元素的相同类型的数组，类型为Type[]</font>
* **`void`** trimToSize() : <font color="#2383CE">对此向量的容量进行微调，使其等于向量的当前大小</font>

## Stack（栈）

### **定义**

栈是Vector的一个子类，它实现了一个标准的**后进先出**的栈

### **方法**

* **`boolean`** empty() :  <font color="#2383CE">测试堆栈是否为空</font>
* **`Object`** peek() : <font color="#2383CE">返回堆栈顶部的对象，但不从堆栈中移除它</font>
* **`Object`** pop() : <font color="#2383CE">弹出堆栈顶部的对象</font>
*  **`int`** search(**`Object`** element) : <font color="#2383CE">返回对象在堆栈中的位置，以</font><font color="#2383CE"><font color="#F0001">**1**</font>为基数</font>

## Dictionary（字典）

### **定义**

给出键和值，就可以将值存储在Dictionary对象中。一旦该值被存储，就可以通过它的键来获取它。所以和Map一样，Dictionary 也可以作为一个键/值对列表。

## Hashtable

### **简例**

```java
Hashtable table = new Hashtable();
table.put("A", 1.0);
table.put("B", 2.0);
table.put("Z", 26.0);
Enumeration names = table.keys();
while(names.hasMoreElements())
{
    String str = names.nextElement().toString();
    System.out.println(str+":"+table.get(str));
}
```

### **方法**

* **`Enumeration`** elements() : <font color="#2383CE">返回Hashtable中值的枚举</font>
* **`void`** rehash(): <font color="#2383CE">增加此哈希表的容量并在内部对其进行重组，以便更有效地容纳和访问其元素</font>
* **`int`** size() : <font color="#2383CE">返回此Hashtable中的键的数量</font>

## Properties 

### **作用**

Properties继承于Hashtable。表示一个持久的属性集.属性列表中每个键及其对应值都是一个字符串。

例如，在获取环境变量时它就作为System.getProperties()方法的返回值。

### **方法**

* **`String`** getProperty(**`String`** key, **`String`** defaultProperty) : <font color="#2383CE">用指定的键在属性列表中搜索属性，若没有此键，则返回第二个参数</font>，<font color="#FFA561">第二个参数可选</font>
* **`void`** list(**`PrintStream`** streamOut) : <font color="#2383CE">将属性列表输出到指定的输出流</font>
* **`void`** list(**`PrintWriter`** streamOut) : <font color="#2383CE">将属性列表输出到指定的输出流</font>
* **`void`** load(**`InputStream`** streamIn) throws IOException : <font color="#2383CE">从输入流中读取属性列表（键和元素对）</font>
* **`Enumeration`** propertyNames() : <font color="#2383CE">返回此属性列表中存在的所有键的枚举</font>
* **`Object`** setProperty(**`String key`**, **`String`** value) : <font color="#2383CE">调用Hashtable的方法put</font>
* **`void`** store(**`OutputStream`** streamOut, **`String`** description) : <font color="#2383CE">将此 Properties表中的属性列表（键和元素对）写入输出流，并添加介绍</font>



# 集合框架

## ArrayList

### **方法**

* **`ArrayList`** subList(**`int`** fromIndex, **`int`** toIndex) : <font color="#2383CE">截取部分arraylist的元素</font>

* **`void`** ensureCapacity(**`int`** minCapacity) : <font color="#2383CE">设置指定容量大小的 arraylist</font>

* **`boolean`** retainAll(**`Collection`** c) : <font color="#2383CE">保留arraylist中在指定集合中也存在的那些元素，也就是删除指定集合中不存在的那些元素</font>

* **`void`** trimToSize() : <font color="#2383CE">将 arraylist 中的容量调整为数组中的元素个数</font>

* **`void`** removeRange(**`int`** fromIndex, **`int`** toIndex) : <font color="#2383CE">删除 arraylist 中指定索引之间存在的元素</font>

* **`void`** replaceAll() : <font color="#2383CE">将给定的操作内容替换掉数组中每一个元素</font>

* **`boolean`** removeIf(**`Predicate<E>`** filter) : <font color="#2383CE">删除所有满足特定条件的 arraylist 元素</font>，<font color="#FFA561">（filter - 过滤器，判断元素是否要删除）</font>

  ~~~java
  ArrayList<String> sites = new ArrayList<>();
  sites.add("Google");
  sites.add("Runoob");
  sites.add("Taobao");
  // 删除名称中带有 Tao 的元素
  sites.removeIf(e -> e.contains("Tao"));;
  ~~~

  

* **`void`** forEach(**`Consumer<E>`** action) : <font color="#2383CE">遍历 arraylist 中每一个元素并执行特定操作</font>，<font color="#FFA561">（action - 对每个元素执行的操作）</font>

  ~~~java
  ArrayList<Integer> numbers = new ArrayList<>();
  numbers.add(1);
  numbers.add(2);
  numbers.add(3);
  numbers.add(4);
  numbers.forEach((e) -> {
  	e = e * 10;
  	System.out.print(e + " ");
  });
  ~~~

## LinkedList

### **方法**

* **`Iterator`** descendingIterator() : <font color="#2383CE">返回倒序迭代器</font>
* **`terator`** listIterator(**`int`** index) : <font color="#2383CE">返回从指定位置开始到末尾的迭代器</font>
* **`Object[]`** toArray() : <font color="#2383CE">返回一个由链表元素组成的数组</font>
* **`Type[]`** toArray(**`Type[]`** a) : <font color="#2383CE">回一个由链表元素转换类型而成的数组，存储在a中</font>

## HashSet

### **定义**

HashSet 基于 HashMap 来实现的，是一个不允许有重复元素的集合。

## HashMap

### **简例**

~~~java
HashMap<Integer, String> Sites = new HashMap<Integer, String>();
Sites.put(1, "Google");
Sites.put(2, "Runoob");
Sites.put(3, "Taobao");
Sites.put(4, "Zhihu");
System.out.println(Sites);
~~~

### **方法**

* **`int`** size() : <font color="#2383CE">返回此Hashmap中的键的数量</font>

* **`void`** .putIfAbsent(**`Object`** key, **`Object`** value) : <font color="#2383CE">如果 hashMap 中不存在指定的键，则将指定的键/值对插入到hashMap中</font>

* **`void`** replaceAll(**`Bifunction<K, V>`** function) : <font color="#2383CE">将hashMap中的所有映射关系替换成给定的函数所执行的结果</font>

* **`Object`** getOrDefault(**`Object`** key) : <font color="#2383CE">获取指定key对应对value，如果找不到key，则返回设置的默认值</font>

* **`void`** forEach(**`Bifunction<K, V>`** function) : <font color="#2383CE">对 HashMap 中的每个映射执行指定的操作</font><font color="#F00001">(该方法不会改变Hashmap中的值)</font>

  ~~~java
  HashMap<String, Integer> prices = new HashMap<>();
  prices.put("Shoes", 200);
  prices.put("Bag", 300);
  prices.put("Pant", 150);
  System.out.println("Normal Price: " + prices);
  System.out.print("Discounted Price: ");
  prices.forEach((key, value) -> {
  	value = value - value * 10/100;
  	System.out.print(key + "=" + value + " ");
  });
  ~~~

* **`Set<Entry<K, V>>`** entrySet() : <font color="#2383CE">返回 hashMap中所有映射项的集合集合视图</font>

  ~~~java
  HashMap<Integer, String> sites = new HashMap<>();
  sites.put(1, "Google");
  sites.put(2, "Runoob");
  sites.put(3, "Taobao");
  System.out.println("sites HashMap: " + sites);
  System.out.println("Set View: " + sites.entrySet());
  ~~~

  ***结果***

  ~~~java
  Set View: [1=Google, 2=Runoob, 3=Taobao]
  ~~~

* **`Set<Object>`** keySet() : <font color="#2383CE">返回 hashMap 中所有 key 组成的集合视图</font>

* **`Object`** compute(**`Object`** key, **`BiFunction`** Function) : <font color="#2383CE">hashMap中指定key的值进行重新计算，如果key对应的value不存在，则返回null，如果存在，则返回通过Function重新计算后的值</font>

* **`Object`** computeIfAbsent(**`Object`** key, **`BiFunction`** Function) : <font color="#2383CE">对hashMap中指定key的值进行重新计算，如果key对应的value不存在，则返回Function重新计算后的值，并保存为value，否则返回value</font>

* **`Object`** computeIfPresent(**`Object`** key, **`BiFunction`** Function) : <font color="#2383CE">对hashMap中指定key的值进行重新计算，前提是该key存在于hashMap中，如果key对应的value不存在，则返回null，如果存在，则返回通过Function重新计算后的值</font>

# 接口

## Object

### **介绍**

Object 类是所有类的父类，也就是说 Java 的所有类都继承了 Object，**子类可以使用 Object 的所有方法**。

### **方法**

* **`Object`** clone() : <font color="#2383CE">创建并返回一个对象的拷贝</font>
* **`boolean`** equals(**`Object`** obj) : <font color="#2383CE">比较两个对象是否相等</font>
* **`void`** finalize() : <font color="#2383CE">当GC(垃圾回收器)确定不存在对该对象的有更多引用时，由对象的垃圾回收器调用此方法</font>
* **`Class<T>`** getClass() : <font color="#2383CE">获取对象的运行时对象的类</font>
* **`int`** hashCode() : <font color="#2383CE">获取对象的 hash 值</font>
* **`void`** notify() : <font color="#2383CE">唤醒在该对象上等待的某个线程</font>
* **`void`** notifyAll() : <font color="#2383CE">唤醒在该对象上等待的所有线程</font>
* **`String`** toString() : <font color="#2383CE">返回对象的字符串表示形式</font>
* **`void`** wait() : <font color="#2383CE">让当前线程进入等待状态。直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法</font>
* **`void`** wait(**`long`** timeout) : <font color="#2383CE">让当前线程处于等待(阻塞)状态，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过参数设置的timeout超时时间</font>
* **`void`** wait(**`long`** timeout, **`int`** nanos) : <font color="#2383CE">与 wait(long timeout) 方法类似，多了一个 nanos 参数，这个参数表示额外时间（以纳秒为单位，范围是 0-999999）。所以超时的时间还需要加上 nanos 纳秒</font>

## 泛型

### **简例**

~~~java
public static < E > void printArray( E[] inputArray )
{          
	for ( E element : inputArray ){        
		System.out.printf( "%s ", element );
    }
	System.out.println();
}
public static void main( String args[] )
{
	Integer[] intArray = { 1, 2, 3, 4, 5 };
	Double[] doubleArray = { 1.1, 2.2, 3.3, 4.4 };
	Character[] charArray = { 'H', 'E', 'L', 'L', 'O' };
    System.out.println( "整型数组元素为:" );
    printArray( intArray  ); // 传递一个整型数组
    System.out.println( "\n双精度型数组元素为:" );
    printArray( doubleArray ); // 传递一个双精度型数组
    System.out.println( "\n字符型数组元素为:" );
    printArray( charArray ); // 传递一个字符型数组
} 
~~~

### **标记**

- **E**：Element (在集合中使用，因为集合中存放的是元素)
- **T**：Type（Java 类）
- **K**：Key（键）
- **V**：Value（值）
- **N**：Number（数值类型）
- **？**：表示不确定的 java 类型
