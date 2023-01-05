## 1.基础

1.一个源文件**只能申明一个public的类**，其他类数目不限，同时文件名一定与public类名称相同。

2.编译器忽略空格。

## 2.标识符

1.标识符必须以字母，下划线，美元符“$”，其他部分必须是数字，字母，下划线，美元符。(字母包含了汉字，但不建议)

2.表示类名的标识符每个单词首字母都需要  大写。

3.**驼峰原则**：表示方法或者变量的标识符，第一个单词小写，从第二个单词开始首字母大写。

## 3.final的用法

一般赋值为变量，能改变其值；而加上final后变为常量。不可修改。

```c++
int a=5;
a=2;
//正确，a为变量，其值可变

final int a=5;
a=2;
//错误，final使得a变为常量，不可再变化。
```

## 4.数据类型

###4.1整数类型

八进制：以0开头  ；

十六进制： 以0x或者0X开头  ；

二进制：以0b或者 0B 开头

### 4.2浮点数类型

浮点数是不精确的，不可用于比较，可用java.math宝下面的两个有用的类，BigInteger 和 BigDecimal。前者可用于任意精度的整数运算，后者可实现任意精度的浮点数运算。

### 4.3布尔类型

boolean

## 5.运算符

1.+：字符串连接符：能将字符串和数字连接起来，也可将字符串连接起来，注意不能连接字符，字符会转化为ASCII码值进行计算。

2.优先级：逻辑非  >   逻辑与  >   逻辑或

## 6.Scanner

获得键盘输入

```java
import java.util.Scanner;

public class Scannertest {
    public static void main (String[] args){
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入你的名字：");
        String name = scanner.nextLine();
        System.out.println("请输入你的爱好：");;
        String favor = scanner.nextLine();
        System.out.println("请输入你的年龄：");
        int age = scanner.nextInt();

        System.out.println("##############");
        System.out.println(name);
        System.out.println(favor);
        System.out.println(name+"的年龄是"+age);
    } 
}
```

[测试代码](/home/peter/javacode/Mypro2_Scanner/Scannertest.java)





## 7.Math类

### 7.1 Math 类的常用方法

ceil()         不小于的最大整数

floor()      不大于的最小整数

round()    最接近的整数

pow()       N的m次方

squrt()      某个数的平方根

abs()        绝对值

### 7.2Random类

random是Math包里的一个函数，可返回  [0,1)  之间的一个随机数。

```java
double d = Math.random();
System.out.println(d);
```

[参考代码]()

## 8.带标签的break和continue

通常用于嵌套循环之中，在continue后面接标签，跳到指定位置，与 goto 有点像。

```java
//打印101到150之间的所有质数
public static void main(String[] args){
    outer:for(int i = 101;i < 150;i++){
        for(int j = 2; j < i/2 ; j++){
            if(i%j == 0){
                continue outer;
            }
        }
        System.out.println(i+" ");
    }
}
```

## 10.方法

java的方法类似于其他语言的函数。

格式：

```java
[修饰符1  修饰符2 ...]  返回类型  方法名(形式参数列表){
    java语句:.............
}
```

### 10.1重载方法：

方法名相同，但形参不同。

## 11.面向过程和面向对象

面向过程适合简单，不需要协作的事务；但是当我们思考比较复杂的问题，需要协作的时候，就要需要面向对象。这两种东西都是解决问题的思维方式，这些都是代码组织的方式。

对象其实就是 **内存块**，里面存放了一些变量和方法 。

类是对象的模板，对象是抽象的东西，所以具体到类。比如天使是对象，类是对天使属性抽象出来的集合，图片中的真实的天使是实例。

类中的属性就是成员变量，类中的方法是函数。

​                                                                                             

## 12.构造方法

构造方法也叫构造器，用于初始化变量。

###12.1使用方法

1.构造方法的名字一定与类的名称相同。

2.通过 new 关键字调用。

3.构造器虽然有返回值，但不能定义返回值类型，不能在构造器中使用return 返回某个值，（可以有return)。

4.如果我们没有定义构造器，则编译器会自动定义一个无参的构造函数。

### 12.2构造方法的重载

与方法的重载是一样的。



## 13.垃圾回收机制

java能自动发现无用的对象，并回收该对象占用的空间。

很多算法来实现这些功能。

### 13.1通用的分代垃圾回收机制

由于不同的对象的生命周期是不一样的，因此，不同生命周期的对象可以采用不同的回收算法。我们将对象分为三种状态：年轻代(Eden/Survivor )，年老代(Tenured/Old)，持久代。

Minor GC：清除年轻代区域

Major GC:  清除老年代区域

Full  GC：清除年轻代，老年代区域，成本较高，会对系统性能产生影响。

### 13.2垃圾回收过程

1.新创建的对象，大多会放进 Eden中 。

2.Eden到了一定比例就会触发垃圾回收 （ GC ） ，将无用的对象清理掉。剩余的对象会被放进 Survivor（假设是S1）中，同时清空 Eden 区域。

3.当Eden空间再次满了  ，会将 S1 无法清除的对象放到另一个   Survivor （S2） 中，同时将S1清空，将Eden中无法清除的对象放到 S1 中并清空 Eden。

4.重复多次（默认15次） Survivor 中没有被清除掉的对象将会被复制到老年区 Old 中。

5.当老年区  (Old)  满了之后，会进行一次完整的垃圾回收 （FullGC)。



程序中使用   System.gc   ，  实际上是程序建议 Full GC 启动，不是直接调用。



## 14.this 的本质

### 14.1 对象创建的四步

1.分配对象空间，并将对象初始化为 0 或者 空。

2.执行属性值的显示初始化。

3.执行构造方法。

4.返回对象的地址给相关的变量。

###14.2作用：

1.可以在类的函数内部用  this.x  区分成员变量和局部变量。

2.可以加在方法前面表明是调用当前类的方法。

3.调用构造方法，直接用 this(x,y) 即可调用 ， 注意必须要位于调用方法的第一句的位置。

###14.3注意：

this 不可用于static静态方法.

原因：非静态方法能调用静态变量，但静态方法无法调用非静态变量或方法。

静态方法可以直接用类来调用，而非静态方法必须先实例化，再用实例来调用。

[参考代码](/home/peter/javacode/MyPro3_static/Statictest.java)



## 15.参数传递机制

方法中的传递其实是地址传递，可以直接在方法内更改原来的元素的值。

但如果我在方法内new了一个，则是对这个元素新建了一片空间，不会影响本来元素的值。

[实践代码](/home/peter/javacode/MyPro4_canshu/canshu.java)



## 16.包机制

1.包机制是  Java  管理类中的重要手段，包对于类实际上就是文件夹对于文件的关系。

2.package 加包 ， 后面接域名（倒着写）。

3.写项目时非注释性语句的第一句一定是包。

4.java中有很多自带的包 ， 其中 java.lang 是最核心的 ，并且这个包不需要导入就可以直接使用。



## 17.import

1.如果要用到另一个包中的某个类 ， 就需要用 import 来导入这个类/包 。

2.也可以在每个引入包的地方采用  包的域名.类名  来引入该类。

3.java中,当一个类的某个成员变量前面不带任何权限修饰时，默认的访问权限只限于包内，此时如果在其他包内导入了这个包，则无法引用这个变量；需要在原来定义这个变量前面加上public修饰。

[参考代码](/home/peter/javacode/MyPro5_package/packagetest.java)

静态导入(static import)是用于导入指定类的静态属性，这样我们就可以直接使用静态属性。



## 18.面向对象

### 18.1 继承

用 extends 就可以继承某个类中的所有变量和方法，同时也可以在继承中继续添加其他变量和方法。

注意事项：

1. 父类也称 超类 ，基类 ， 派生类 。

2. java 中只有单继承 ， 不像C++中有多继承。

3. java 中类没有多继承 ， 但是接口有多继承 。 

4. 子类继承父类 ， 可以得到父类的全部属性和方法 （除了父类的构造方法），但不见得可以直接访问，（比如父类的私有属性和方法）。

5. 如果定义一个类时，没有调用extends，则他的父类是： java.lang.Object

6. ==instanceof==  是二元运算符 ，左边是对象 ，右边是类 ； 当对象是右边的类 或者 它的子类所创建的对象时 ， 返回 true ；否则返回 false   。

    [参考代码](/home/peter/javacode/MyPro6_jicheng/JiCheng.java)

### 18.2  Object 类

object 类是所有类的根 ，属于  java.lang

to.String ( ) 方法 属于Object 类，他返回了一个字符串。我们可以重写它。



## 19.  重写自带方法

1.若是变量 == ，则数值相同 ；若是引用 == ，则为地址相同。

2.equals 默认判断二者地址是否相同，我们可以选择重写。..

[参考代码](/home/peter/javacode/MyPro7_chongxie/Chongxie.java)



## 20.  super()

1.super是直接父类对象的引用。可以通过super来访问父类中被子类覆盖的方法或者属性。

2.super()永远位于构造器的第一句 ， 作用是调用父类构造器，一直上溯到Object，然后再以此向下执行类的初始化和构造方法。

3.寻找属性或者方法时，首先在当前类中寻找，若找不到，再到它的父类中去找，层层递进，直到Object。

[参考代码](/home/peter/javacode/MyPro8_super/Super.java)



## 21.封装；

### 21.1访问控制符

1.  private:同一个类。
2. default:同一个包，也是默认的访问控制符。
3. protected:子类。
4. public: 同一个项目。

除了能修饰属性和方法，还可以修饰类。

### 21.2使用细节

1.属性变量一般都用 private，方法用public 。一般用 setxxxx   ,   getxxxx 等方法来访问。这样我们可以在设置变量的时候在方法内设置边界。

​                           

## 22.多态

多态是指对于不同的对象，同一个方法会有不同的行为。

### 22.1多态的要求

1.多态是方法的多态，不是属性的多态。

2.多态有三个必要条件：继承 ， 方法重写 ， 父类引用指向子类对象 。

3.父类引用指向子类对象后，用该父类引用调用子类重写的方法，此时多态就出现了。

### 22.2理解

其实就是一个父类有多个子类，子类中都存在对父类一个方法的重写。这样我们在主类中定义一个方法，引用父类，就可以在main函数中通过调用该方法传入不同的子类对象实现不同的方法调用 。 这种目的其实我们也可以通过不断写函数重构来实现，但显然函数重构过于麻烦。

[参考代码](/home/peter/javacode/MyPro9_duotai/Duotai.java)



## 23.转型

Java可以自动向上转型 ， 也就是子类自动转换为父类 ，但若要父类转换为子类 ，需要设置强制转型。注意同级之间是不可以强制转换的，编译能过但运行会报错。



## 24.final

final 可以修饰 变量 ， 方法 和 类。

变量加 final后变成常量 ， 值无法改变。

方法加 final后可以被重载，无法被重写。

类加 final 后不能被子类继承 ，如 Math , String等等。





## 25.引用类型和基本类型

基本类型包括 number  string boolean undefined null

引用类型包括 array  object  function

区别主要在以下三个方面：

1.内存空间：内存分为栈和堆 ， 基本类型存放于栈内存中 ， 包括变量以及变量的值 ；引用类型大小不固定，但他们的地址大小固定，所以把地址存在栈中而值存在堆中。

2.对值的操作：基本类型按值访问 ， 而引用类型按引用访问 ， 会在栈中重新创建一个新值 ， 然后把值复制到为新变量分配的位置上。

3.变量的复制：基本类型的复制会在栈中创建一个新值 ， 然后把值复制到为新变量分配的位置上。而引用类型变量的复制是复制储存在栈中的指针，将指针复制到栈中为新变量分配的空间中，这两个指针指向的其实是堆中的同一个对象，复制操作结束后，两个变量实际上将引用同一个对象，改变一个会影响其中另一个。



## 26.数组

###26.1声明：

1.    类型[ ]  数组名；
2.    类型     数组名[ ];

### 26.2初始化

静态初始化：声明的同时进行初始化。

动态初始化：先声明，再通过下标进行赋值。

### 26.3遍历

可用循环遍历，也可用 foreach 循环 ， 用于读取数组元素的值 ， 不能修改元素的值。

[参考代码](/home/peter/javacode/MyPro10_shuzu/shuzu.java)

### 26.4 数组克隆

testBasicCopy(被克隆数组，起始被克隆位置，克隆数组，起始克隆位置，克隆长度)；

可以借用这个 函数 实现数组克隆，数组删除，数组插入。数组换位等一系列操作。

[参考代码](/home/peter/javacode/MyPro10_shuzu/Aaraycopy.java)

### 26.5  数组常用API

Arrays.toString()              输出整个数组

sort()                                  数组排序

binarySearch()                  二分法查找       

### 26.6  二维数组

二维数组有两种定义方式

auto\[][]     a  =  new   auto\[][]

 [参考代码	]()	

## 27.抽象方法和抽象类

### 27.1抽象方法

使用 abstract  修饰的方法，没有方法体，只有声明。

### 27.2抽象类

包含抽象方法的类就是抽象类，通过   abstract  方法定义规范。它的作用是为子类提供统一的，规范的模板。子类必须实现相关的抽象方法。

### 27.3抽象类的使用要点

1.有抽象方法的类只能定义为抽象类。

2.抽象类不能实例化，既不能用  new 来实例化抽象类。

3.抽象类可以包含属性，方法，构造方法。但是构造方法不能用来 new 实例，只能用来被子类调用。

4.抽象类只能用来被继承。

5.抽象方法必须由子类实现。



## 28.接口

###28.1定义

接口是比抽象类更加抽象的抽象类，接口里面不含有任何实现，**所有方法都是抽象方法**，它定义了一系列的规范。接口和实现类之间不是父子关系，是实现规则的关系，体现了“如果你是……那么你一定能……” 的思想。

###28.2申明格式

```java
[访问修饰符]  interface  接口名  [extends  父接口1 ， 父接口2.....]{
    常量定义；
    方法定义；
}
```

### 28.3定义接口的详细说明

1.接口里面不能定义变量，只能定义常量，默认加  public static final 修饰。方法默认  public   abstract 修饰。

2.接口可以继承，并且可以多继承。

### 28.4优点

通过面向接口编程，而不是面向实现类编程，可以大大降低程序模块之间的耦合性，提高整个系统的可扩展性和可维护性。

[参考代码](/home/peter/javacode/MyPro13_Abstract)



## 29.内部类的分类

java中内部类分类主要有成员内部类（静态内部类，非静态内部类），匿名内部类，局部内部类。

### 29.1成员内部类

#### 29.1.1非静态

a)非静态内部类一定即存在一个外部类对象里。

b)**非静态内部类可以直接访问外部类成员，但是外部类不能访问非静态内部类成员。**

c)非静态内部类不能有静态方法，静态属性和静态初始化块。

d)外部类的静态方法，静态代码类不能访问非静态内部类，包括不能使用非静态内部类定义变量，创建实例。

e)成员变量访问要点：

1.内部类方法中的成员变量：  直接用变量名即可

2.内部类属性： this.变量名

3.外部类属性：外部类名.this.变量名

#### 29.1.2静态

```java
//定义方式
static  class  ClassName {
    //类体
}
```

.使用要点

a)当一个静态内部类对象存在时，并**不一定存在对应的外部类对象**。因此静态内部类无法直接访问外部类的方法。

b)静态内部类看作外部类的一个静态变量，因此，外部类的方法中可以通过：“静态内部类.名字”的方式访问静态内部类的静态成员，通过  new  静态内部类的实例。

```java
class  Outer{
    //相当于外部类的一个静态成员
    static class Inner{
    }
}
public class TestStaticInnerClass{
    public static void main(String[] args){
        //通过  new  外部类名.内部类名() 来创建内部类对象
        Outer.Inner  innner = new Outer.Inner();
    }
}
```

### 29.2匿名内部类

适合那种**只需要使用一次的类**。比如：键盘监听操作等等。

```java
new 父类构造器(实参类表\实现接口(){
    //匿名内部类类体!
}
```

注意事项：

1.匿名内部类**没有访问修饰符**。

2.匿名内部类**没有构造方法**。

### 29.3局部内部类

定义在方法内部，作用域仅限于本方法，成为局部内部类。

```java
public class Test{
    public void show(){
        //作用域仅限于该方法
        class Inner{
            public void fun(){
                System.out.println("helloworld");
            }
        }
        new INner().fun();
    }
    public static void main(String[] args){
        new Test().show();
    }
}
```

[参考代码](/home/peter/javacode/MyPro14_neibulei)

## 30.String 类

### 30.1  String基础

String 类又称作不可变字符序列，String位于java.lang中。java字符串就是Unicode字符拼接而成。

### 30.2 String 类和常量池

用引号直接赋值的字符串对象都是直接指向常量池里面的内容，而采用new方法得到的字符串是另外开辟的空间。

### 30.3 String 常用API

charAt()                  返回字符串中指定位置的字符

length()                   返回字符串的长度

equals()                  比较两个字符串

equalsIgnoreCase()      比较两个字符串，忽略大小写

indexOf()                查看字符串中是否存在某个子字符串        

startsWith()            判断字符串是否以XXX为开头

endsWith()            判断字符串是否以XXX为结束

substring()             将字符串中指定范围的字符复制给另一个字符串

toLowerCase()         转小写

toUpperCase()       转大写

trim()                      去除字符串首尾的空字符    

### 30.4 String 不可变

String 的内核是 private fina  char  value[];   所以是常量  

字符串比较时，用  equals  不要用 ==

### 30.5 StringBuilder  可修改

StringBuffer  线程安全，效率低；

StringBuilder   线程不安全，效率高（常用）

### 30.6 StringBuilder 常用API

append()     //字符串后面接上字符串

reverse()     //颠倒字符串

setCharAt()  //插入字符

insert()         //插入字符，可链式插入

delete()      //删除区域内的字符，可链式删除

## 31.排序

### 31.1  冒泡排序

[参考代码](/home/peter/javacode/MyPro16_maopao/Maopao_paixu.java)



## 32. 包装类

java是面向对象的语言，但并不是完全面向对象的，比如我们用到的基本数据类型就不是面向对象的，我们在运用时需要将基本类型转化为对象便于操作，为了解决这个不足，**java在设计类时为每个基本数据类型设计了一个对应的类进行代表，称之为==包装类==**。

###32.1 基本数据类型对应的包装类

| 基本数据类型 |  包装类   |
| :----------: | :-------: |
|     byte     |   Byte    |
|   boolean    |  Boolean  |
|    short     |   Short   |
|     char     | Character |
|     int      |  Integer  |
|     long     |   Long    |
|    float     |   Float   |
|    double    |  Double   |

### 32.2 自动装/拆箱

JAVA 有自动装箱 和 自动拆箱功能，即能够自动在基本类型和包装类之间转换。

[参考代码](/home/peter/javacode/MyPro17_Baozhuang)



## 33 . Date  时间处理类

[参考代码](/home/peter/javacode/MyPro18_DateTest)

### 33.1 初始化

Date()    分配一个  Date  类 ，它的对象表示一个特定的瞬间。

Date(long  date)  分配Date对象并初始化此对象，以表示自从标准基准时间以来的指定毫秒数。

boolean  after(Date  when)   测试此时间是否在指定时间之后

before()     是否在之前

getTime()     毫秒数

### 33.2  DateFormat类 和 SimpleDateFormat 类

 DateFormat 类的作用是实现字符串和时间对象之间的相互转换，它是一个抽象类，我们平常使用它的实现类 SimpleDateFormat。

[参考代码](/home/peter/javacode/MyPro18_DateTest/SimpleDateFormattest.java)

### 33.3 Calendar 日历类

Calendar 是一个抽象类 ， 为我们提供了关于日期计算的相关功能。比如：年，月，日，时，分，秒的展示和计算。

GregorianCalendar 是 Calendar 的一个具体子类，提供了世界上大多数国家/地区使用的标准日历系统。

注意：一月是0开始的，父类Calendar常用常量来表示月份：JANUARY，FEBRUARY。

get() 获得日期

set() 设置日期

通过Calendar对象可以获得对应的年月日，星期几等信息。

[参考代码](/home/peter/javacode/MyPro18_DateTest/CalendarTest.java)

### 33.4 日历

[参考代码](/home/peter/javacode/MyPro18_DateTest/Calendartest.java)

 



​                
