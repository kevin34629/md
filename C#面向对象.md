# **C#面向对象**

## **类**

### **调用**

#### **非静态类**

**非静态类使用时应先实例化。**

~~~C#
Test0 test = new Test0(); //将类Test0实例化。
test.Out(); //调用Test0类内的Out()方法。
~~~

#### **静态类**

<font color="#FF0000">**#静态类仅包含静态字段，且不能包含实例构造函数。**</font>

~~~C#
Test0.Out();//无需实例化，直接调用Test0中的Out方法。
~~~

**使用类时**

### **属性**

~~~c#
class Test1
{
    private string n = "AAA"; //默认值为“AAA”
    public string N
    {
        get { return n; } //访问N时返回n的值
        set 
        { if(value != "") { n= value; } } //当输入不为空值时将值写n
    }
}
~~~

引用目标时调用get访问器获取目标的值，当目标作为被赋值对象引用时调用set访问器，set访问器含有隐式参数value。

<font color="#FF0000">**#当目标只包含get访问器时为只读属性，只包含set访问器时为只写属性。**</font>

### **构造函数与析构函数**

~~~C#
class Test3 
{
    static Test3() { Console.WriteLine("@"); } //静态构造函数
    public Test3(string a) //构造函数
    {
        Console.Write("AT ");
        Console.WriteLine(a);
    }
    ~Test3() { Console.WriteLine("end"); } //析构函数
}
~~~

#当定义类时，若没有定义构造函数编译器会自动生产一个不带参的构造函数，若已定义了含参的构造函数，若仍想使用默认构造函数，则需进行显式定义

**#静态构造函数仅会在第一次使用类时执行一次，<font color="#FF0000">用于从外部源初始化类内的静态字段和属性</font>，一个类中只能有一个静态构造函数，且其<font color="#2383CE">只能访问类内的静态成员，不能访问实例成员</font>。**

构造函数会在每次类被实例化时执行一次，当一个类的**实例**不再有效时，便会调用析构函数。

**#析构函数常用于进行<font color="#FFA561">垃圾回收</font>，通常自动调用，无需显式定义，<font color="#FF0000">且在.NET中应避免使用析构函数释放资源</font>。**

<font color="#21A8F0">*实现 Finalize 方法或析构函数对性能可能会有负面影响，因此应避免不必要地使用它们。用 Finalize 方法回收对象使用的内存需要至少两次垃圾回收。当垃圾回收器执行回收时，它只回收没有终结器的不可访问对象的内存。这时，它不能回收具有终结器的不可访问对象。它改为将这些对象的项从终止队列中移除并将它们放置在标为准备终止的对象列表中。该列表中的项指向托管堆中准备被调用其终止代码的对象。垃圾回收器为此列表中的对象调用 Finalize 方法，然后，将这些项从列表中移除。后来的垃圾回收将确定终止的对象确实是垃圾，因为标为准备终止对象的列表中的项不再指向它们。在后来的垃圾回收中，实际上回收了对象的内存。*</font>

### **方法**

#### **参数**

1. **值参数**
2. **ref参数：引用传递，<font color="#FF0000">需要变量在传递前赋值</font>，<font color="#2383CE">使用时方法和方法调用都需要显示使用ref关键字</font>。**
3. **out参数：引用传递，<font color="#FF0000">无需变量在传递前赋值</font>，<font color="#2383CE">使用时方法和方法调用都需要显示使用out关键字</font>。**
4. **params参数：用于传递多个相同类型的参数，本质上为一维数组,<font color="#2383CE">主要用于指定在参数数目可变时采用的方法参数</font>。**

<font color="#FF0000">**#定义方法时其可选参数及params参数必须处于参数列表末尾且不能同时出现。**</font>

####  **虚方法**

虚方法使用**<font color="#FF0000">virtual</font>**关键字定义，在父类中其不能被访问<font color="#2383CE">**仅能通过派生实现**</font>。

子类可使用**<font color="#FF0000">override</font>**重写父类的虚方法，也能使用<font color="#FF0000">**base**</font>关键字调用重写前的方法。

~~~C#
class Ftest0
{
    public Ftest0() { Console.WriteLine("F"); }
    public virtual void Test0() { Console.WriteLine("F"); }
}
class Stest0 : Ftest0
{
    public Stest0() { Console.WriteLine("S"); }
    public override void Test0() { Console.WriteLine("S"); } //重写Test0()
    public void Test1() { base.Test0(); } //调用父类Test0()方法
}
~~~


### **静态成员**

<font color="#2383CE">**当类内成员定义为静态成员时无需实例化即可直接对其进行访问。**</font>

~~~C#
Test0.Out();//调用Test0类内的Out()方法。
~~~

### **权限修饰符**

1. **private:仅在本类中访问。**
2. **protected:仅在本类和子类中访问。**
3. **internal:在同一程序集中访问。**
4. **protected internal:在同一程序集和子类中访问。**
5. **public:任何代码都可访问。**

### **继承**

**继承时子类继承父类的所有方法，但也能使用<font color="#FF0000">new</font>关键字重写父类方法。**

~~~C#
class Ftest0
{
    public Ftest0() { Console.WriteLine("F"); }
    public void Test0() { Console.WriteLine("F"); }
}
class Stest0 : Ftest0 //继承Ftest0
{
    public Stest0() { Console.WriteLine("S"); }
    public new void Test0() { Console.WriteLine("S"); } //重写Test0()
    public void Test1() { base.Test0(); } //调用父类Test0()方法
}
~~~

**#父类方法被重写后如需调用则需要使得<font color="#FF0000">base</font>关键字。**

**<font color="#2383CE">#当类使用sealed关键字时，其为密封类，不支持继承。</font>**

### **抽象类**

抽象类使用<font color="#FF0000">**abstract**</font>关键字定义，**<font color="#2383CE">抽象类无法被实例化，仅能通过派生实现</font>，与接口类似**。

~~~C#
abstract class Atest0 { abstract public void Test0(); } //定义抽象类
lass Atest1 : Atest0 //派生（继承）
{
    public Atest1() { Console.WriteLine("Atest1"); }
    public override void Test0() { Console.WriteLine("Atest1"); }//实现抽象类中的方法
}
~~~

## **接口**

**接口使用<font color="#FF0000">interface</font>关键字声明，接口只包含<font color="#2383CE">成员的声明</font>,其功能均要通过派生来具体实现。**

**<font color="#2383CE">#接口定义了所有类继承接口时应遵循的语法合同,使得实现接口的类或结构在形式上保持一致。</font>**

**<font color="#FF0000">#接口支持多重继承。</font>**

~~~C#
interface Itest0 { void Test0(); } //定义接口Itest0
interface Itest1 : Itest0 { void Test1(); } //定义接口Itest1,并继承自Itest0
class Itest2 : Itest1 //定义类Itest2，继承Itest1,类中同时包含Test0()和Test1()
{
    public void Test0() { Console.WriteLine("ITest0"); }
    public void Test1() { Console.WriteLine("ITest1"); }
}
~~~

**<font color="#FF0000">#一个类也能继承多个接口。</font>**

上面的代码等效于下方的代码。

~~~C#
interface Itest0 { void Test0(); }
interface Itest1 : Itest0 { void Test1(); }
class Itest2 : Itest1,Itest0 //定义类Itest2，同时继承Itest0，Itest1,类中同时包含Test0()和Test1()
{
    public void Test0() { Console.WriteLine("ITest0"); }
    public void Test1() { Console.WriteLine("ITest1"); }
}
~~~

## **结构体**

**结构体**使用<font color="#FF0000">**struct**</font>关键字定义

- 结构可带有方法、字段、索引、属性、运算符方法和事件。
- 结构可定义构造函数，但不能定义析构函数。<font color="#FF0000">**不能为结构定义无参构造函数**</font>。无参构造函数是自动定义的，且**不能被改变**。
- 与类不同，结构不能继承其他的结构或类。
- 结构不能作为其他结构或类的基础结构。
- 结构可实现一个或多个接口。
- <font color="#2383CE">**结构成员不能指定为 abstract、virtual 或 protected。**</font>
- 当您使用**new**操作符创建一个结构对象时，会调用适当的构造函数来创建结构。与类不同，结构可以不使用**new**操作符即可被实例化。
- 如果不使用**new**操作符，只有在所有的字段都被初始化之后，字段才被赋值，对象才被使用。
- <font color="#FF0000">**结构体中声明的字段无法赋予初值。**</font>
- <font color="#FF0000">**结构体的构造函数中，必须为结构体所有字段赋值。**</font>

~~~C#
class Program 
{
    void Main() 
    {
        Book book0 = new Book();//使用new操作符实例化
        book0.tiltle = "Test";
        book0.author = "None";
        Book book1;//不使用new操作符实例化
        book1.tiltle = "Test";
        book1.author = "None";
    }
}
struct Book 
{
    public string tiltle;
    public string author;
}
~~~

