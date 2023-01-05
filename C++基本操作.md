#  

## 1.string

### 1.1构造函数

```c++
string();	   //构造一个空的字符串string s1
string(const string &str);	//构造一个与str一样的string。如string s1(s2)。

// 带参数的构造函数
string(const char *s);    //用字符串s初始化
string(int n,char c);    //用n个字符c初始化
```

### 1.2string的存取字符操作

```c++
const char &operator[] (int n) const;
const char &at(int n) const;
char &operator[] (int n);
char &at(int n);

//例子：
char c = strA[3];
char d = strA.at(5);

strA[3] = 'X';
strA.at(5) = 'Y';
```

operator[]和at()均返回当前字符串中第n个字符，但二者是有区别的
主要区别在于**at()在越界时会==抛出异常==， []在刚好越界时会返回 (char)0，再继续越界时，编译器直接出错** 。
如果你的程序希望可以通过 try...catch捕获异常，建议采用 at()。 

```c++
int length() const;   //返回当前字符串的长度。长度不包括字符串结尾的'\0'。
bool empty() const;     //当前字符串是否为空

int copy(char *s, int n, int pos=0) const;     //把当前串中以pos开始的n个字符拷贝到以s为起始位置的字符数组中，返回实际拷贝的数目。注意要保证s所指向的空间足够大以容纳当前字符串，不然会越界。

```

### 1.3字符串的连接

```c++
string &operator+=(const string &s);    //把字符串s连接到当前字符串结尾
string &operator+=(const char *s);      //把字符串s连接到当前字符串结尾
string &append(const char *s);          //把字符串s连接到当前字符串结尾
string &append(const char *s,int n);    //把字符串s的前n个字符连接到当前字符串结尾
string &append(const string &s);        //同operator+=()
string &append(const string &s,int pos, int n);//把字符串s中从pos开始的n个字符连接到当前字符串结尾
string &append(int n, char c);          //在当前字符串结尾添加n个字符c
```

### 1.4字符串的赋值

```c++
string &operator=(const string &s);                //把字符串s赋给当前的字符串
string &assign(const char *s);                     //把字符串s赋给当前的字符串
string &assign(const char *s, int n);              //把字符串s的前n个字符赋给当前的字符串
string &assign(const string &s);                   //把字符串s赋给当前字符串
string &assign(int n,char c);                      //用n个字符c赋给当前字符串
string &assign(const string &s,int start, int n);  //把字符串s中从start开始的n个字符赋给当前字符串
```

### 1.5string的比较

```c++
int compare(const string &s) const;  //与字符串s比较
int compare(const char *s) const;   //与字符串s比较
```

### 1.6string的查找

```c++
int find(char c,int pos=0) const;  //从pos开始查找字符c在当前字符串的位置 
int find(const char *s, int pos=0) const;  //从pos开始查找字符串s在当前字符串的位置
int find(const string &s, int pos=0) const;  //从pos开始查找字符串s在当前字符串中的位置

//rfind是反向查找的意思
int rfind(char c, int pos=npos) const;   //从pos开始从后向前查找字符c在当前字符串中的位置 
int rfind(const char *s, int pos=npos) const;
int rfind(const string &s, int pos=npos) const;
```

查找不到，返回-1

### 1.7string的插入

```c++
string &insert(int pos, const char *s);
string &insert(int pos, const string &s);
// 前两个函数在pos位置插入字符串s
string &insert(int pos, int n, char c);  // 在pos位置 插入n个字符c
```

###1.8string的删除

```c++
string &erase(int pos=0, int n=npos);  //删除pos开始的n个字符，返回修改后的字符串
```

###1.9string的替换

```c++
string &replace(int pos, int n, const char *s);//删除从pos开始的n个字符，然后在pos处插入串s
string &replace(int pos, int n, const string &s);  //删除从pos开始的n个字符，然后在pos处插入串s
void swap(string &s2);    //交换当前字符串与s2的值
```

## 2stringstream类介绍

###2.1 概述

<sstream\> 定义了三个类：istringstream、ostringstream 和 stringstream，分别用来进行流的输入、输出和输入输出操作。本文以 stringstream 为主，介绍流的输入和输出操作。

<sstream\> 主要用来进行数据类型转换，由于 <sstream\> 使用 string 对象来代替字符数组（snprintf 方式），避免了缓冲区溢出的危险；而且，因为传入参数和目标对象的类型会被自动推导出来，所以不存在错误的格式化符号的问题。简单说，相比 C 编程语言库的数据类型转换，<sstream\> 更加安全、自动和直接。

### 2.2 数据类型转化

这里展示一份示例代码，介绍将 int 类型转换为 string 类型的过程。

```c++
#include <string>
#include <sstream>
#include <iostream>
#include <stdio.h>
 
using namespace std;
 
int main()
{
    stringstream sstream;
    string strResult;
    int nValue = 1000;
 
    // 将int类型的值放入输入流中
    sstream << nValue;
    // 从sstream中抽取前面插入的int类型的值，赋给string类型
    sstream >> strResult;
 
    cout << "[cout]strResult is: " << strResult << endl;
    printf("[printf]strResult is: %s\n", strResult.c_str());
 
    return 0;
}
```

编译并执行上述代码，结果如下:

![img](https://img-blog.csdn.net/20180910235107617?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpaXRkYXI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 2.2多个字符的拼接

本示例介绍在 stringstream 中存放多个字符串，实现多个字符串拼接的目的（其实完全可以使用 string 类实现），同时，介绍 stringstream 类的清空方法。

示例代码（stringstream_test2.cpp）的内容如下：

```c++
#include <string>
#include <sstream>
#include <iostream>
 
using namespace std;
 
int main()
{
    stringstream sstream;
 
    // 将多个字符串放入 sstream 中
    sstream << "first" << " " << "string,";
    sstream << " second string";
    cout << "strResult is: " << sstream.str() << endl;
 
    // 清空 sstream
    sstream.str("");
    sstream << "third string";
    cout << "After clear, strResult is: " << sstream.str() << endl;
 
    return 0;
}
```

编译并执行上述代码，结果如下：

![img](https://img-blog.csdn.net/20180911001717178?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpaXRkYXI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

从上述代码执行结果能够知道：

   可以使用 str() 方法，将 stringstream 类型转换为 string 类型；
   可以将多个字符串放入 stringstream 中，实现字符串的拼接目的；
   如果想清空 stringstream，必须使用 sstream.str(""); 方式；clear() 方法适用于进行多次数据类型转换的场景。详见示例 2.3。

### 2.3stringstream的清空

清空 stringstream 有两种方法：clear() 方法以及 str("") 方法，这两种方法对应不同的使用场景。str("") 方法的使用场景，在上面的示例中已经介绍过了，这里介绍 clear() 方法的使用场景。

示例代码（stringstream_test3.cpp）的内容如下：

```c++
#include <sstream>
#include <iostream>
 
using namespace std;
 
int main()
{
    stringstream sstream;
    int first, second;
 
    // 插入字符串
    sstream << "456";
    // 转换为int类型
    sstream >> first;
    cout << first << endl;
 
    // 在进行多次类型转换前，必须先运行clear()
    sstream.clear();
 
    // 插入bool值
    sstream << true;
    // 转换为int类型
    sstream >> second;
    cout << second << endl;
 
    return 0;
}
```

编译并执行上述代码，结果如下：

![img](https://img-blog.csdn.net/20180911004449489?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpaXRkYXI=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



##3.C++保留字

**1.asm**

asm (指令字符串)：允许在 C++ 程序中嵌入汇编代码。

**2. auto**

auto（自动，automatic）是存储类型标识符，表明变量"自动"具有本地范围，块范围的变量声明（如for循环体内的变量声明）默认为auto存储类型。

**3. bool**

bool（布尔）类型，C++ 中的基本数据结构，其值可选为 true（真）或者 false（假）。C++ 中的 bool 类型可以和 int 混用，具体来说就是 0 代表 false，非 0 代表 true。bool 类型常用于条件判断和函数返回值。

**4. break**

break（中断、跳出），用在switch语句或者循环语句中。程序遇到 break 后，即跳过该程序段，继续后面的语句执行。

**5. case**

用于 switch 语句中，用于判断不同的条件类型。

**6. catch**

catch 和 try 语句一起用于异常处理。

**7. char**

char（字符，character）类型，C++ 中的基本数据结构，其值一般为 0~255 的 int。这 256 个字符对应着 256 个 ASCII 码。char 类型的数据需要用单引号 **'** 括起来。

**8.class**

class（类）是 C++ 面向对象设计的基础。使用 class 关键字声明一个类。

**9. const**

const（常量的，constant）所修饰的对象或变量不能被改变，修饰函数时，该函数不能改变在该函数外面声明的变量也不能调用任何非const函数。在函数的声明与定义时都要加上const，放在函数参数列表的最后一个括号后。在 C++ 中，用 const 声明一个变量，意味着该变量就是一个带类型的常量，可以代替 #define，且比 #define 多一个类型信息，且它执行内链接，可放在头文件中声明；但在 C 中，其声明则必须放在源文件（即 .C 文件）中，在 C 中 const 声明一个变量，除了不能改变其值外，它仍是一具变量。如:

```
const double pi(3.14159);
或 
const double pi = 3.14159;
```



**10. const_cast用法：**

```
const_cast<type_id> (expression)
```

该运算符用来修改类型的 const 或 volatile 属性。除了 const 或 volatile 修饰之外， type_id 和 expression 的类型是一样的。常量指针被转化成非常量指针，并且仍然指向原来的对象；常量引用被转换成非常量引用，并且仍然指向原来的对象；常量对象被转换成非常量对象。

**11. continue**

continue（继续）关键字用于循环结构。它使程序跳过代码段后部的部分，与 break 不同的是，continue 不是进入代码段后的部分执行，而是重新开始新的循环。因而它是"继续循环"之意，不是 break（跳出）。

**12. default**

default（默认、缺省）用于 switch 语句。当 switch 所有的 case 都不满足时，将进入 default 执行。default 只能放在 switch 语句所有的 case 之后，并且是可选的。

**13. delete**

delete（删除）释放程序动态申请的内存空间。delete 后面通常是一个指针或者数组 []，并且只能 delete 通过 new 关键字申请的指针，否则会发生段错误。

**14. do**

do-while是一类循环结构。与while循环不同，do-while循环保证至少要进入循环体一次。

**15. double**

double（双精度）类型，C++ 中的基本数据结构，以双精度形式存储一个浮点数。

**16. dynamic_cast**

dynamic_cast（动态转换），允许在运行时刻进行类型转换，从而使程序能够在一个类层次结构安全地转换类型。dynamic_cast 提供了两种转换方式，把基类指针转换成派生类指针，或者把指向基类的左值转换成派生类的引用。

**17. else**

else 紧跟在 if 后面，用于对 if 不成立的情况的选择。

**18. enum**

enum（枚举）类型，给出一系列固定的值，只能在这里面进行选择一个。

**19. explicit**

explicit（显式的）的作用是"禁止单参数构造函数"被用于自动型别转换，其中比较典型的例子就是容器类型。在这种类型的构造函数中你可以将初始长度作为参数传递给构造函数。

**20. export**

为了访问其他编译单元（如另一代码文件）中的变量或对象，对普通类型（包括基本数据类、结构和类），可以利用关键字 extern，来使用这些变量或对象时；但是对模板类型，则必须在定义这些模板类对象和模板函数时，使用标准 C++ 新增加的关键字 export（导出）。

**21. extern**

extern（外部的）声明变量或函数为外部链接，即该变量或函数名在其它文件中可见。被其修饰的变量（外部变量）是静态分配空间的，即程序开始时分配，结束时释放。用其声明的变量或函数应该在别的文件或同一文件的其它地方定义（实现）。在文件内声明一个变量或函数默认为可被外部使用。在 C++ 中，还可用来指定使用另一语言进行链接，这时需要与特定的转换符一起使用。目前仅支持 **C** 转换标记，来支持 C 编译器链接。使用这种情况有两种形式：

```
extern "C" 声明语句

extern "C" { 声明语句块 }
```



**22. false**

false（假的），C++ 的基本数据结构 bool 类型的值之一。等同于 int 的 0 值。

**23. float**

float（浮点数），C++ 中的基本数据结构，精度小于 double。

**24. for**

for 是 C++ 中的循环结构之一。

**25. friend**

friend（友元）声明友元关系。友元可以访问与其有 friend 关系的类中的 private/protected 成员，通过友元直接访问类中的 private/protected 成员的主要目的是提高效率。友元包括友元函数和友元类。

**26. goto**

goto（转到），用于无条件跳转到某一标号处开始执行。

**27. if**

if（如果），C++ 中的条件语句之一，可以根据后面的 bool 类型的值选择进入一个分支执行。

**28. inline**

inline（内联）函数的定义将在编译时在调用处展开。inline 函数一般由短小的语句组成，可以提高程序效率。

**29. int**

int（整型，integer），C++ 中的基本数据结构，用于表示整数，精度小于 long。

**30. long**

long（长整型，long integer），C++ 中的基本数据结构，用于表示长整数。

**31. mutable**

mutable（易变的）是 C++ 中一个不常用的关键字。只能用于类的非静态和非常量数据成员。由于一个对象的状态由该对象的非静态数据成员决定，所以随着数据成员的改变，对像的状态也会随之发生变化。如果一个类的成员函数被声明为 const 类型，表示该函数不会改变对象的状态，也就是该函数不会修改类的非静态数据成员。但是有些时候需要在该类函数中对类的数据成员进行赋值，这个时候就需要用到 mutable 关键字。

**32. namespace**

namespace（命名空间）用于在逻辑上组织类，是一种比类大的结构。

**33. new**

new（新建）用于新建一个对象。new 运算符总是返回一个指针。由 new 创建

**34. operator**

operator（操作符）用于操作符重载。这是 C++ 中的一种特殊的函数。

**35. private**

private（私有的），C++ 中的访问控制符。被标明为 private 的字段只能在本类以及友元中访问。

**36. protected**

protected（受保护的），C++ 中的访问控制符。被标明为 protected 的字段只能在本类以及其继承类和友元中访问。

**37. public**

public（公有的），C++ 中的访问控制符。被标明为 public 的字段可以在任何类

**38.register**

register（寄存器）声明的变量称着寄存器变量，在可能的情况下会直接存放在机器的寄存器中；但对 32 位编译器不起作用，当 global optimizations（全局优化）开的时候，它会做出选择是否放在自己的寄存器中；不过其它与 register 关键字有关的其它符号都对32位编译器有效。

**39. reinterpret_cast**

用法：

```
reinpreter_cast<type-id> (expression)
```

type-id 必须是一个指针、引用、算术类型、函数指针或者成员指针。它可以把一个指针转换成一个整数，也可以把一个整数转换成一个指针（先把一个指针转换成一个整数，在把该整数转换成原类型的指针，还可以得到原先的指针值）。

**40. return**

return（返回）用于在函数中返回值。程序在执行到 return 语句后立即返回，return 后面的语句无法执行到。

**41. short**

short（短整型，short integer），C++ 中的基本数据结构，用于表示整数，精度小于 int。

**42. signed**

signed（有符号），表明该类型是有符号数，和 unsigned 相反。数字类型（整型和浮点型）都可以用 signed 修饰。但默认就是 signed，所以一般不会显式使用。

**43. sizeof**

由于 C++ 每种类型的大小都是由编译器自行决定的，为了增加可移植性，可以用 sizeof 运算符获得该数据类型占用的字节数。

**44. static**

static（静态的）静态变量作用范围在一个文件内，程序开始时分配空间，结束时释放空间，默认初始化为 0，使用时可改变其值。静态变量或静态函数，只有本文件内的代码才可访问它，它的名字（变量名或函数名）在其它文件中不可见。因此也称为"文件作用域"。在 C++ 类的成员变量被声明为 static（称为静态成员变量），意味着它被该类的所有实例所共享，也就是说当某个类的实例修改了该静态成员变量，其修改值为该类的其它所有实例所见；而类的静态成员函数也只能访问静态成员（变量或函数）。类的静态成员变量必须在声明它的文件范围内进行初始化才能使用，private 类型的也不例外。

**45. static_cast**

用法：

```
static_cast < type-id > ( expression ) 
```

该运算符把 expression 转换为 type-id 类型，但没有运行时类型检查来保证转换的安全性。它主要有如下几种用法：

- ① 用于类层次结构中基类和子类之间指针或引用的转换。进行上行转换（把子类的指针或引用转换成基类表示）是安全的；进行下行转换（把基类指针或引用转换成子类表示）时，由于没有动态类型检查，所以是不安全的。
- ② 用于基本数据类型之间的转换，如把 int 转换成 char，把 int 转换成 enum。这种转换的安全性也要开发人员来保证。
- ③ 把空指针转换成目标类型的空指针。
- ④ 把任何类型的表达式转换成void类?

**注意** static_cast 不能转换掉 expression 的 const、volitale、或者 __unaligned 属性。

**46. struct**

struct（结构）类型，类似于 class 关键字，与 C 语言兼容（class 关键字是不与 C 语言兼容的），可以实现面向对象程序设计。

**47. switch**

switch（转换）类似于 if-else-if 语句，是一种多分枝语句。它提供了一种简洁的书写，并且能够生成效率更好的代码。但是，switch 后面的判断只能是int（char也可以，但char本质上也是一种int类型）。switch 语句最后的 default 分支是可选的。

**48. template**

template（模板），C++ 中泛型机制的实现。

**49. this**

this 返回调用者本身的指针。

**50. throw**

throw（抛出）用于实现 C++ 的异常处理机制，可以通过 throw 关键字"抛出"一个异常。

**51. true**

true（真的），C++ 的基本数据结构 bool 类型的值之一。等同于 int 的非 0 值。

**52. try**

try（尝试）用于实现 C++ 的异常处理机制。可以在 try 中调用可能抛出异常的函数，然后在 try 后面的 catch 中捕获并进行处理。

**53. typedef**

typedef（类型定义，type define），其格式为：

```
typedef  类型 定义名;
```

类型说明定义了一个数据类型的新名字而不是定义一种新的数据类型。定义名表示这个类型的新名字。

**54. typeid**

指出指针或引用指向的对象的实际派生类型。

**55. typename**

typename（类型名字）关键字告诉编译器把一个特殊的名字解释成一个类型。在下列情况下必须对一个 name 使用 typename 关键字：

- 1． 一个唯一的name（可以作为类型理解），它嵌套在另一个类型中的。
- 2． 依赖于一个模板参数，就是说：模板参数在某种程度上包含这个name。当模板参数使编译器在指认一个类型时产生了误解。

**56. union**

union（联合），类似于 enum。不同的是 enum 实质上是 int 类型的，而 union 可以用于所有类型，并且其占用空间是随着实际类型大小变化的。

**57. unsigned**

unsigned（无符号），表明该类型是无符号数，和 signed 相反。

**58. using**

表明使用 namespace。

**59. virtual**

virtual（虚的），C++ 中用来实现多态机制。

**60. void**

void（空的），可以作为函数返回值，表明不返回任何数据；可以作为参数，表明没有参数传入（C++中不是必须的）；可以作为指针使用。

**61. volatile**

volatile（不稳定的）限定一个对象可被外部进程（操作系统、硬件或并发线程等）改变，声明时的语法如下：

```
int volatile nVint;
```

这样的声明是不能达到最高效的，因为它们的值随时会改变，系统在需要时会经常读写这个对象的值。因此常用于像中断处理程序之类的异步进程进行内存单元访问。

**62. wchar_t**

wchar_t 是宽字符类型，每个 wchar_t 类型占 2 个字节，16 位宽。汉字的表示就要用到 wchar_t。

## 4.数据类型范围

| 类型               | 位            | 范围                                                         |
| :----------------- | :------------ | :----------------------------------------------------------- |
| char               | 1 个字节      | -128 到 127 或者 0 到 255                                    |
| unsigned char      | 1 个字节      | 0 到 255                                                     |
| signed char        | 1 个字节      | -128 到 127                                                  |
| int                | 4 个字节      | -2147483648 到 2147483647                                    |
| unsigned int       | 4 个字节      | 0 到 4294967295                                              |
| signed int         | 4 个字节      | -2147483648 到 2147483647                                    |
| short int          | 2 个字节      | -32768 到 32767                                              |
| unsigned short int | 2 个字节      | 0 到 65,535                                                  |
| signed short int   | 2 个字节      | -32768 到 32767                                              |
| long int           | 8 个字节      | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807      |
| signed long int    | 8 个字节      | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807      |
| unsigned long int  | 8 个字节      | 0 到 18,446,744,073,709,551,615                              |
| float              | 4 个字节      | 精度型占4个字节（32位）内存空间，+/- 3.4e +/- 38 (~7 个数字) |
| double             | 8 个字节      | 双精度型占8 个字节（64位）内存空间，+/- 1.7e +/- 308 (~15 个数字) |
| long double        | 16 个字节     | 长双精度型 16 个字节（128位）内存空间，可提供18-19位有效数字。 |
| wchar_t            | 2 或 4 个字节 | 1 个宽字符                                                   |

## 5.枚举类型

枚举类型(enumeration)是C++中的一种派生数据类型，它是由用户定义的若干枚举常量的集合。

**如果一个变量只有几种可能的值，可以定义为枚举(enumeration)类型**。所谓"枚举"是指将变量的值一一列举出来，变量的值只能在列举出来的值的范围内。

创建枚举，需要使用关键字 ==enum==。枚举类型的一般形式为：

```c++
enum 枚举名{ 
     标识符[=整型常数], 
     标识符[=整型常数], 
... 
    标识符[=整型常数]
} 枚举变量;
    
```

如果枚举没有初始化, 即省掉"=整型常数"时, 则从第一个标识符开始。

例如，下面的代码定义了一个颜色枚举，变量 c 的类型为 color。最后，c 被赋值为 "blue"。

```c++
enum color { red, green, blue } c;
c = blue;
```

默认情况下，第一个名称的值为 0，第二个名称的值为 1，第三个名称的值为 2，以此类推。但是，您也可以给名称赋予一个特殊的值，只需要添加一个初始值即可。例如，在下面的枚举中，**green** 的值为 5。

```
enum color { red, green=5, blue };
```

**在这里，blue 的值为 6，因为默认情况下，每个名称都会比它前面一个名称大 1，但 red 的值依然为 0。**

## 6.全局变量初始化

当局部变量被定义时，系统不会对其初始化，您必须自行对其初始化。定义全局变量时，系统会自动初始化为下列值：

| 数据类型 | 初始化默认值 |
| :------- | :----------- |
| int      | 0            |
| char     | '\0'         |
| float    | 0            |
| double   | 0            |
| pointer  | NULL         |

## 7.常量

###7.1 整数常量

整数常量可以是十进制、八进制或十六进制的常量。前缀指定基数：==0x 或 0X 表示十六进制，0 表示八进制，不带前缀则默认表示十进制==。

整数常量也可以带一个后缀，==后缀是 U 和 L 的组合，U 表示无符号整数（unsigned），L 表示长整数（long）==。后缀可以是大写，也可以是小写，U 和 L 的顺序任意。

###7.2 布尔常量

布尔常量共有两个，它们都是标准的 C++ 关键字：

- **true** 值代表真。
- **false** 值代表假。

我们不应把 true 的值看成 1，把 false 的值看成 0。

###7.3 字符常量

字符常量是括在单引号中。如果常量**以 L（仅当大写时）开头**，则表示它是一个宽字符常量（例如 L'x'），此时它必须存储在 **wchar_t** 类型的变量中。否则，它就是一个窄字符常量（例如 'x'），此时它可以存储在 **char** 类型的简单变量中。

字符常量可以是一个普通的字符（例如 'x'）、一个转义序列（例如 '\t'），或一个通用的字符（例如 '\u02C0'）。

在 C++ 中，有一些特定的字符，当它们前面有反斜杠时，它们就具有特殊的含义，被用来表示如换行符（\n）或制表符（\t）等。下表列出了一些这样的转义序列码：

| 转义序列   | 含义                       |
| :--------- | :------------------------- |
| \\         | \ 字符                     |
| \'         | ' 字符                     |
| \"         | " 字符                     |
| \?         | ? 字符                     |
| \a         | 警报铃声                   |
| \b         | 退格键                     |
| \f         | 换页符                     |
| \n         | 换行符                     |
| \r         | 回车                       |
| \t         | 水平制表符                 |
| \v         | 垂直制表符                 |
| \ooo       | 一到三位的八进制数         |
| \xhh . . . | 一个或多个数字的十六进制数 |

### 7.4定义常量

\#define    预处理器

const   关键字

## 8.储存类

###8.1 auto 存储类

自 C++ 11 以来，**auto** 关键字用于两种情况：声明变量时根据初始化表达式自动推断该变量的类型、声明函数时函数返回值的占位符。

C++98标准中auto关键字用于自动变量的声明，但由于使用极少且多余，在 C++17 中已删除这一用法。

根据初始化表达式自动推断被声明的变量的类型，如：

```c++
auto f=3.14;      //double 
auto s("hello");  //const char* 
auto z = new auto(9); // int* 
auto x1 = 5, x2 = 5.0, x3='r';//错误，必须是初始化为同一类型
```

###8.2 register 存储类

**register** 存储类用于定义存储在寄存器中而不是 RAM 中的局部变量。这意味着变量的最大尺寸等于寄存器的大小（通常是一个词），且不能对它应用一元的 '&' 运算符（因为它没有内存位置）。

```c++
{   
    register int  miles; 
}
```

寄存器只用于需要快速访问的变量，比如计数器。还应注意的是，定义 'register' 并不意味着变量将被存储在寄存器中，它意味着变量可能存储在寄存器中，这取决于硬件和实现的限制。

###8.3 static 存储类

**static** 存储类指示编译器在程序的生命周期内保持局部变量的存在，而不需要在每次它进入和离开作用域时进行创建和销毁。因此，使用 static 修饰局部变量可以在函数调用之间保持局部变量的值。

static 修饰符也可以应用于全局变量。当 static 修饰全局变量时，会使变量的作用域限制在声明它的文件内。

在 C++ 中，当 static 用在类数据成员上时，会导致仅有一个该成员的副本被类的所有对象共享。

```c++
#include <iostream>  // 函数声明  
void func(void); 
static int count = 10; /* 全局变量 */ 
int main() {    
    while(count--)    {     
       func();  
    } 
    return 0;
} // 函数定义 
void func( void ) {  
    static int i = 5; // 局部静态变量  
    i++;   
    std::cout << "变量 i 为 " << i ; 
    std::cout << " , 变量 count 为 " << count << std::endl; 
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```
变量 i 为 6 , 变量 count 为 9
变量 i 为 7 , 变量 count 为 8
变量 i 为 8 , 变量 count 为 7
变量 i 为 9 , 变量 count 为 6
变量 i 为 10 , 变量 count 为 5
变量 i 为 11 , 变量 count 为 4
变量 i 为 12 , 变量 count 为 3
变量 i 为 13 , 变量 count 为 2
变量 i 为 14 , 变量 count 为 1
变量 i 为 15 , 变量 count 为 0
```

### 8.4 extern 存储类

**extern** 存储类用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的。当您使用 'extern' 时，对于无法初始化的变量，会把变量名指向一个之前定义过的存储位置。

当您有多个文件且定义了一个可以在其他文件中使用的全局变量或函数时，可以在其他文件中使用 *extern* 来得到已定义的变量或函数的引用。可以这么理解，*extern* 是用来在另一个文件中声明一个全局变量或函数。

extern 修饰符通常用于当有两个或多个文件共享相同的全局变量或函数的时候，如下所示：

第一个文件：main.cpp

```c++
#include <iostream> 
int count ;
extern void write_extern(); 
int main() { 
    count = 5;  
    write_extern();
}
```

```c++
#include <iostream> 
extern int count; 
void write_extern(void) 
{ 
    std::cout << "Count is " << count << std::endl;
}
```

在这里，第二个文件中的 *extern* 关键字用于声明已经在第一个文件 main.cpp 中定义的 count。现在 ，编译这两个文件，如下所示：

```c++
$ g++ main.cpp support.cpp -o write
```

这会产生 **write** 可执行程序，尝试执行 **write**，它会产生下列结果：

```c++
$ ./write
Count is 5
```

###8.5 mutable 存储类

**mutable** 说明符仅适用于类的对象，这将在本教程的最后进行讲解。它允许对象的成员替代常量。也就是说，mutable 成员可以通过 const 成员函数修改。

###8.6thread_local 存储类

使用 thread_local 说明符声明的变量仅可在它在其上创建的线程上访问。 变量在创建线程时创建，并在销毁线程时销毁。 每个线程都有其自己的变量副本。

thread_local 说明符可以与 static 或 extern 合并。

可以将 thread_local 仅应用于数据声明和定义，thread_local 不能用于函数声明或定义。

以下演示了可以被声明为 thread_local 的变量：

```c++
thread_local int x;  // 命名空间下的全局变量 
class X {    
    static thread_local std::string s; // 类的static成员变量 }; 
    static thread_local std::string X::s;  // X::s 是需要定义的  
    void foo() {    thread_local std::vector<int> v;  // 本地变量 
}
```

## 9.函数

### 9.1Lambda函数表达式

C++11 提供了对匿名函数的支持,称为 Lambda 函数(也叫 Lambda 表达式)。

Lambda 表达式把函数看作对象。Lambda 表达式可以像对象一样使用，比如可以将它们赋给变量和作为参数传递，还可以像函数一样对其求值。

Lambda 表达式本质上与函数声明非常类似。Lambda 表达式具体形式如下:

```c++
[capture](parameters)->return-type{body}
```

例如：

```c++
[](int x, int y){ return x < y ; }
```

如果没有返回值可以表示为：

```c++
[capture](parameters){body}
```

例如：

```c++
[]{ ++global_x; } 
```

在一个更为复杂的例子中，返回类型可以被明确的指定如下：

```c++
[](int x, int y) -> int { int z = x + y; return z + x; }
```

本例中，一个临时的参数 z 被创建用来存储中间结果。如同一般的函数，z 的值不会保留到下一次该不具名函数再次被调用时。

如果 lambda 函数没有传回值（例如 void），其返回类型可被完全忽略。

在Lambda表达式内可以访问当前作用域的变量，这是Lambda表达式的闭包（Closure）行为。 与JavaScript闭包不同，C++变量传递有传值和传引用的区别。可以通过前面的[]来指定：

```c++
[]      // 沒有定义任何变量。使用未定义变量会引发错误。
[x, &y] // x以传值方式传入（默认），y以引用方式传入。
[&]     // 任何被使用到的外部变量都隐式地以引用方式加以引用。
[=]     // 任何被使用到的外部变量都隐式地以传值方式加以引用。
[&, x]  // x显式地以传值方式加以引用。其余变量以引用方式加以引用。
[=, &z] // z显式地以引用方式加以引用。其余变量以传值方式加以引用。
```

另外有一点需要注意。对于[=]或[&]的形式，lambda 表达式可以直接使用 this 指针。但是，对于[]的形式，如果要使用 this 指针，必须显式传入：

```c++
[this]() { this->someFunc(); }();
```

### 9.2参数的默认值

当您定义一个函数，您可以为参数列表中后边的每一个参数指定默认值。当调用函数时，如果实际参数的值留空，则使用这个默认值。

这是通过在函数定义中使用赋值运算符来为参数赋值的。**调用函数时，如果未传递参数的值，则会使用默认值，如果指定了值，则会忽略默认值，使用传递的值**。请看下面的实例：

```c++
#include <iostream> 
using namespace std;  
int sum(int a, int b=20) 
{ 
    int result;  
    result = a + b;    
    return (result); 
}  
int main () 
{   
 // 局部变量声明  
    int a = 100; 
    int b = 200; 
    int result;    
    // 调用函数来添加值   
    result = sum(a, b);   
    cout << "Total value is :" << result << endl; 
    // 再次调用函数   
    result = sum(a); 
    cout << "Total value is :" << result << endl;   
    return 0; 
}
```





当上面的代码被编译和执行时，它会产生下列结果：

```c++
Total value is :300
Total value is :120
```



###9.3数学运算函数

在 C++ 中，除了可以创建各种函数，还包含了各种有用的函数供您使用。这些函数写在标准 C 和 C++ 库中，叫做==内置函数==。您可以在程序中引用这些函数。

C++ 内置了丰富的数学函数，可对各种数字进行运算。下表列出了 C++ 中一些有用的内置的数学函数。

为了利用这些函数，您需要引用==数学头文件 <cmath\>==。

| 函数                              | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| **double cos(double);**           | 该函数返回弧度角（double 型）的余弦。                        |
| **double sin(double);**           | 该函数返回弧度角（double 型）的正弦。                        |
| **double tan(double);**           | 该函数返回弧度角（double 型）的正切。                        |
| **double log(double);**           | 该函数返回参数的自然对数。                                   |
| **double pow(double, double);**   | 假设第一个参数为 x，第二个参数为 y，则该函数返回 x 的 y 次方。 |
| **double hypot(double, double);** | 该函数返回两个参数的平方总和的平方根，也就是说，参数为一个直角三角形的两个直角边，函数会返回斜边的长度。 |
| **double sqrt(double);**          | 该函数返回参数的平方根。                                     |
| **double floor(double);**         | 该函数返回一个小于或等于传入参数的最大整数。                 |
|                                   |                                                              |

```c++
#include <iostream>
#include <cmath>
using namespace std;
 
int main ()
{
   // 数字定义
   short  s = 10;
   int    i = -1000;
   long   l = 100000;
   float  f = 230.47;
   double d = 200.374;
 
   // 数学运算
   cout << "sin(d) :" << sin(d) << endl;
   cout << "abs(i)  :" << abs(i) << endl;
   cout << "floor(d) :" << floor(d) << endl;
   cout << "sqrt(f) :" << sqrt(f) << endl;
   cout << "pow( d, 2) :" << pow(d, 2) << endl;
 
   return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```c++
sin(d) :-0.634939
abs(i)  :1000
floor(d) :200
sqrt(f) :15.1812
pow( d, 2 ) :40149.7
```



## 10.随机数

在许多情况下，需要生成随机数。关于随机数生成器，有两个相关的函数。一个是 **rand()**，**该函数只返回一个伪随机数**。生成随机数之前必须先调用 **srand()** 函数。

下面是一个关于生成随机数的简单实例。**实例中使用了time() 函数来获取系统时间的秒数，通过调用 rand() 函数来生成随机数：**

```c++
#include <iostream>
#include <ctime>
#include <cstdlib>
 
using namespace std;
 
int main ()
{
   int i,j;
 
   // 设置种子
   srand( (unsigned)time( NULL ) );
 
   /* 生成 10 个随机数 */
   for( i = 0; i < 10; i++ )
   {
      // 生成实际的随机数
      j= rand();
      cout <<"随机数： " << j << endl;
   }
 
   return 0;
}
```

##11.日期  &  时间

C++ 标准库没有提供所谓的日期类型。C++ 继承了 C 语言用于日期和时间操作的结构和函数。为了使用日期和时间相关的函数和结构，需要在 C++ 程序中引用 ==<ctime\> 头文件==。

有四个与时间相关的类型：**clock_t、time_t、size_t** 和 **tm**。类型 clock_t、size_t 和 time_t 能够把系统时间和日期表示为某种整数。

结构类型 **tm** 把日期和时间以 C 结构的形式保存，tm 结构的定义如下：

```c++
struct tm { 
    int tm_sec;   // 秒，正常范围从 0 到 59，但允许至 61  
    int tm_min;   // 分，范围从 0 到 59 
    int tm_hour;  // 小时，范围从 0 到 23  
    int tm_mday;  // 一月中的第几天，范围从 1 到 31 
    int tm_mon;   // 月，范围从 0 到 11 
    int tm_year;   // 自 1900 年起的年数  
    int tm_wday;  // 一周中的第几天，范围从 0 到 6，从星期日算起  
    int tm_yday;  // 一年中的第几天，范围从 0 到 365，从 1 月 1 日算起
    int tm_isdst; // 夏令时 
};
```

下面是 C/C++ 中关于日期和时间的重要函数。所有这些函数都是 C/C++ 标准库的组成部分，您可以在 C++ 标准库中查看一下各个函数的细节。

[链接](https://www.runoob.com/cplusplus/cpp-date-time.html)

## 12.基本的输入输出流

C++ 的 I/O 发生在流中，流是字节序列。如果字节流是从设备（如键盘、磁盘驱动器、网络连接等）流向内存，这叫做**输入操作**。如果字节流是从内存流向设备（如显示屏、打印机、磁盘驱动器、网络连接等），这叫做**输出操作**。

| <iostream\> | 该文件定义了 **cin、cout、cerr** 和 **clog** 对象，分别对应于标准输入流、标准输出流、非缓冲标准错误流和缓冲标准错误流。 |
| ----------- | ------------------------------------------------------------ |
| <iomanip\>  | 该文件通过所谓的参数化的流操纵器（比如 **setw** 和 **setprecision**），来声明对执行标准化 I/O 有用的服务。 |
| <fstream\>  | 该文件为用户控制的文件处理声明服务。我们将在文件和流的相关章节讨论它的细节。 |

###12.1标准输出流（cout）

预定义的对象 **cout** 是 **iostream** 类的一个实例。cout 对象"连接"到标准输出设备，通常是显示屏。**cout** 是与流插入运算符 << 结合使用的，如下所示：

```c++
#include <iostream>
 
using namespace std;
 
int main( )
{
   char str[] = "Hello C++";
 
   cout << "Value of str is : " << str << endl;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```c++
Value of str is : Hello C++
```

**C++ 编译器根据要输出变量的数据类型，选择合适的流插入运算符来显示值**。<< 运算符被重载来输出内置类型（整型、浮点型、double 型、字符串和指针）的数据项。

流插入运算符 << 在一个语句中可以多次使用，如上面实例中所示，==**endl** 用于在行末添加一个换行符==。

### 12.2标准输入流（cin）

预定义的对象 **cin** 是 **iostream** 类的一个实例。cin 对象附属到标准输入设备，通常是键盘。**cin** 是与流提取运算符 >> 结合使用的，如下所示：

```c++
#include <iostream>
 
using namespace std;
 
int main( )
{
   char name[50];
 
   cout << "请输入您的名称： ";
   cin >> name;
   cout << "您的名称是： " << name << endl;
}
```

当上面的代码被编译和执行时，它会提示用户输入名称。当用户输入一个值，并按回车键，就会看到下列结果：

```c++
请输入您的名称： cplusplus
您的名称是： cplusplus
```

C++ 编译器根据要输入值的数据类型，选择合适的流提取运算符来提取值，并把它存储在给定的变量中。

流提取运算符 >> 在一个语句中可以多次使用，如果要求输入多个数据，可以使用如下语句：

```c++
cin >> name >> age;
```

这相当于下面两个语句：

```
cin >> name;
cin >> age;
```

###12.3 标准错误流（cerr）

预定义的对象 **cerr** 是 **iostream** 类的一个实例。cerr 对象附属到标准输出设备，通常也是显示屏，但是 **cerr** 对象是非缓冲的，且每个流插入到 cerr 都会立即输出。

**cerr** 也是与流插入运算符 << 结合使用的，如下所示：

```c++
#include <iostream>
 
using namespace std;
 
int main( )
{
   char str[] = "Unable to read....";
 
   cerr << "Error message : " << str << endl;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```c++
Error message : Unable to read....
```

### 12.4标准日志流（clog）

预定义的对象 **clog** 是 **iostream** 类的一个实例。clog 对象附属到标准输出设备，通常也是显示屏，但是 **clog** 对象是缓冲的。这意味着每个流插入到 clog 都会先存储在缓冲区，直到缓冲填满或者缓冲区刷新时才会输出。

**clog** 也是与流插入运算符 << 结合使用的，如下所示：

```c++
#include <iostream>
using namespace std;
int main( ) { 
    char str[] = "Unable to read...."; 
    clog << "Error message : " << str << endl;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```
Error message : Unable to read....
```

通过这些小实例，我们无法区分 cout、cerr 和 clog 的差异，但在编写和执行大型程序时，它们之间的差异就变得非常明显。**所以良好的编程实践告诉我们，使用 cerr 流来显示错误消息，而其他的日志消息则使用 clog 流来输出**。

### 12.5cout标志

```c++
#include <iostream>
#include <iomanip>
using namespace std;
int main()
{
    cout<<setiosflags(ios::left|ios::showpoint);  // 设左对齐，以一般实数方式显示
    cout.precision(5);       // 设置除小数点外有五位有效数字 
    cout<<123.456789<<endl;
    cout.width(10);          // 设置显示域宽10 
    cout.fill('*');          // 在显示区域空白处用*填充
    cout<<resetiosflags(ios::left);  // 清除状态左对齐
    cout<<setiosflags(ios::right);   // 设置右对齐
    cout<<123.456789<<endl;
    cout<<setiosflags(ios::left|ios::fixed);    // 设左对齐，以固定小数位显示
    cout.precision(3);    // 设置实数显示三位小数
    cout<<999.123456<<endl; 
    cout<<resetiosflags(ios::left|ios::fixed);  //清除状态左对齐和定点格式
    cout<<setiosflags(ios::left|ios::scientific);    //设置左对齐，以科学技术法显示 
    cout.precision(3);   //设置保留三位小数
    cout<<123.45678<<endl;
    return 0; 
}
```



## 13.类  &  对象

C++ 在 C 语言的基础上增加了**面向对象编程**，C++ 支持面向对象程序设计。类是 C++ 的核心特性，通常被称为用户定义的类型。

###13.1 类的定义

类用于指定对象的形式，它包含了数据表示法和用于处理数据的方法。**类中的数据和方法称为类的成员。函数在一个类中被称为类的成员。**

![img](https://www.runoob.com/wp-content/uploads/2015/05/cpp-classes-objects-2020-12-10-11.png)

类定义是以关键字 **class** 开头，后跟类的名称。类的主体是包含在一对花括号中。类定义后必须跟着一个分号或一个声明列表。例如，我们使用关键字 **class** 定义 Box 数据类型，如下所示：

```c++
class Box
{
   public:
      double length;   // 盒子的长度
      double breadth;  // 盒子的宽度
      double height;   // 盒子的高度
};
```

###13.2对象的定义

类提供了对象的蓝图，所以基本上，对象是根据类来创建的。声明类的对象，就像声明基本类型的变量一样。下面的语句声明了类 Box 的两个对象：

```c++
Box Box1;          // 声明 Box1，类型为 Box
Box Box2;          // 声明 Box2，类型为 Box
```

对象 Box1 和 Box2 都有它们各自的数据成员。

### 13.3访问数据成员

类的对象的公共数据成员可以使用直接成员访问运算符 **.** 来访问。

```c++
#include <iostream>
 
using namespace std;
 
class Box
{
   public:
      double length;   // 长度
      double breadth;  // 宽度
      double height;   // 高度
      // 成员函数声明
      double get(void);
      void set( double len, double bre, double hei );
};
// 成员函数定义
double Box::get(void)
{
    return length * breadth * height;
}
 
void Box::set( double len, double bre, double hei)
{
    length = len;
    breadth = bre;
    height = hei;
}
int main( )
{
   Box Box1;        // 声明 Box1，类型为 Box
   Box Box2;        // 声明 Box2，类型为 Box
   Box Box3;        // 声明 Box3，类型为 Box
   double volume = 0.0;     // 用于存储体积
 
   // box 1 详述
   Box1.height = 5.0; 
   Box1.length = 6.0; 
   Box1.breadth = 7.0;
 
   // box 2 详述
   Box2.height = 10.0;
   Box2.length = 12.0;
   Box2.breadth = 13.0;
 
   // box 1 的体积
   volume = Box1.height * Box1.length * Box1.breadth;
   cout << "Box1 的体积：" << volume <<endl;
 
   // box 2 的体积
   volume = Box2.height * Box2.length * Box2.breadth;
   cout << "Box2 的体积：" << volume <<endl;
 
 
   // box 3 详述
   Box3.set(16.0, 8.0, 12.0); 
   volume = Box3.get(); 
   cout << "Box3 的体积：" << volume <<endl;
   return 0;
}
 //////////////////////////////////////
Box1 的体积：210
Box2 的体积：1560
Box3 的体积：1536


```

需要注意的是，**私有的成员和受保护的成员不能使用直接成员访问运算符 (.) 来直接访问**。我们将在后续的教程中学习如何访问私有成员和受保护的成员。

###13.4类成员函数

类的成员函数是指那些把定义和原型写在类定义内部的函数，就像类定义中的其他变量一样。**类成员函数是类的一个成员，它可以操作类的==任意==对象，可以访问对象中的==所有==成员。**

```c++
class Box
{
   public:
      double length;         // 长度
      double breadth;        // 宽度
      double height;         // 高度
      double getVolume(void);// 返回体积
};
```

成员函数可以定义在类定义内部，或者单独使用==**范围解析运算符 ::**== 来定义。在类定义中定义的成员函数把函数声明为**内联**的，即便没有使用 inline 标识符。

```c++
class Box
{
   public:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
   
      double getVolume(void)
      {
         return length * breadth * height;
      }
};
///////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////
或者：
double Box::getVolume(void)
{
    return length * breadth * height;
}
```

在这里，需要强调一点，在 :: 运算符之前必须使用类名。

```c++
#include <iostream>
 
using namespace std;
 
class Box
{
   public:
      double length;         // 长度
      double breadth;        // 宽度
      double height;         // 高度
 
      // 成员函数声明
      double getVolume(void);
      void setLength( double len );
      void setBreadth( double bre );
      void setHeight( double hei );
};
 
// 成员函数定义
double Box::getVolume(void)
{
    return length * breadth * height;
}
 
void Box::setLength( double len )
{
    length = len;
}
 
void Box::setBreadth( double bre )
{
    breadth = bre;
}
 
void Box::setHeight( double hei )
{
    height = hei;
}
 
// 程序的主函数
int main( )
{
   Box Box1;                // 声明 Box1，类型为 Box
   Box Box2;                // 声明 Box2，类型为 Box
   double volume = 0.0;     // 用于存储体积
 
   // box 1 详述
   Box1.setLength(6.0); 
   Box1.setBreadth(7.0); 
   Box1.setHeight(5.0);
 
   // box 2 详述
   Box2.setLength(12.0); 
   Box2.setBreadth(13.0); 
   Box2.setHeight(10.0);
 
   // box 1 的体积
   volume = Box1.getVolume();
   cout << "Box1 的体积：" << volume <<endl;
 
   // box 2 的体积
   volume = Box2.getVolume();
   cout << "Box2 的体积：" << volume <<endl;
   return 0;
}
```



### 13.5类的访问修饰符

数据封装是面向对象编程的一个重要特点，它防止函数直接访问类类型的内部成员。类成员的访问限制是通过在类主体内部对各个区域标记 **public、private、protected** 来指定的。关键字 **public、private、protected** 称为访问修饰符。

一个类可以有多个 public、protected 或 private 标记区域。每个标记区域在下一个标记区域开始之前或者在遇到类主体结束右括号之前都是有效的。**成员和类的默认访问修饰符是 private**。

```c++
class Base {
   public:
  // 公有成员
   protected:
  // 受保护成员
   private:
  // 私有成员
};
```

1.**公有**成员在程序中类的外部是可访问的。您可以不使用任何成员函数来设置和获取公有变量的值。

2.**私有**成员变量或函数在类的外部是不可访问的，甚至是不可查看的。只有==类==和==友元函数==可以访问私有成员。

默认情况下，类的所有成员都是私有的。

3.**protected（受保护）**成员变量或函数与私有成员十分相似，但有一点不同，protected（受保护）成员在==派生类（即子类）==中是可访问的。

```c++
#include <iostream>
 
using namespace std;
 
class Box
{
   public:
      double length;
      void setWidth( double wid );
      double getWidth( void );
 
   private:
      double width;
};
 
// 成员函数定义
double Box::getWidth(void)
{
    return width ;
}
 
void Box::setWidth( double wid )
{
    width = wid;
}
 
// 程序的主函数
int main( )
{
   Box box;
 
   // 不使用成员函数设置长度
   box.length = 10.0; // OK: 因为 length 是公有的
   cout << "Length of box : " << box.length <<endl;
 
   // 不使用成员函数设置宽度
   // box.width = 10.0; // Error: 因为 width 是私有的
   box.setWidth(10.0);  // 使用成员函数设置宽度
   cout << "Width of box : " << box.getWidth() <<endl;
 
   return 0;
}
```



**继承中的特点**：

有public, protected, private三种继承方式，它们相应地改变了基类成员的访问属性。

- 1.**public 继承：**基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：public, protected, private
- 2.**protected 继承：**基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：protected, protected, private
- 3.**private 继承：**基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：private, private, private

但无论哪种继承方式，上面两点都没有改变：

- 1.private 成员只能被本类成员（类内）和友元访问，不能被派生类访问；

- 2.protected 成员可以被派生类访问。

  ```c++
  #include<iostream>
  #include<assert.h>
  using namespace std;
   
  class A{
  public:
    int a;
    A(){
      a1 = 1;
      a2 = 2;
      a3 = 3;
      a = 4;
    }
    void fun(){
      cout << a << endl;    //正确
      cout << a1 << endl;   //正确
      cout << a2 << endl;   //正确
      cout << a3 << endl;   //正确
    }
  public:
    int a1;
  protected:
    int a2;
  private:
    int a3;
  };
  class B : public A{
  public:
    int a;
    B(int i){
      A();
      a = i;
    }
    void fun(){
      cout << a << endl;       //正确，public成员
      cout << a1 << endl;       //正确，基类的public成员，在派生类中仍是public成员。
      cout << a2 << endl;       //正确，基类的protected成员，在派生类中仍是protected可以被派生类访问。
      cout << a3 << endl;       //错误，基类的private成员不能被派生类访问。
    }
  };
  int main(){
    B b(10);
    cout << b.a << endl;
    cout << b.a1 << endl;   //正确
    cout << b.a2 << endl;   //错误，类外不能访问protected成员
    cout << b.a3 << endl;   //错误，类外不能访问private成员
    system("pause");
    return 0;
  }
  ```

###13.6 类的构造函数 

#### 13.6.1基础构造函数

类的**构造函数**是类的一种特殊的成员函数，**==它会在每次创建类的新对象时执行==**。

**构造函数的名称与类的==名称是完全相同的==，==并且不会返回任何类型==，也不会返回 void**。构造函数==可用于为某些成员变量设置初始值==。

```c++
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
当上面的代码被编译和执行时，它会产生下列结果：

Object is being created
Length of line : 6
```

#### 13.6.2带参数的构造函数

默认的构造函数没有任何参数，但如果需要，**构造函数也可以带有参数**。这样在创建对象时就会给对象赋初始值，如下面的例子所示：

```c++
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line(double len);  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line( double len)
{
    cout << "Object is being created, length = " << len << endl;
    length = len;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line(10.0);
 
   // 获取默认设置的长度
   cout << "Length of line : " << line.getLength() <<endl;
   // 再次设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}

当上面的代码被编译和执行时，它会产生下列结果：

Object is being created, length = 10
Length of line : 10
Length of line : 6
```

#### 13.6.3使用初始化列表来初始化字段

使用初始化列表来初始化字段：

```c++
Line::Line( double len): length(len)
{
    cout << "Object is being created, length = " << len << endl;
}
```

上面的语法等同于如下语法：

```c++
Line::Line( double len) {
    length = len; 
    cout << "Object is being created, length = " << len << endl; 
}
```

假设有一个类 C，具有多个字段 X、Y、Z 等需要进行初始化，同理地，您可以使用上面的语法，只需要在不同的字段使用逗号进行分隔，如下所示：

```c++
C::C( double a, double b, double c): X(a), Y(b), Z(c)
{
  ....
}
```

### 13.7析构函数

类的**析构函数**是类的一种特殊的成员函数，它会在==每次删除所创建的对象时执行==。

析构函数的==名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，它不会返回任何值，也不能带有任何参数。==析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源。

```c++
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();   // 这是构造函数声明
      ~Line();  // 这是析构函数声明
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
Line::~Line(void)
{
    cout << "Object is being deleted" << endl;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
当上面的代码被编译和执行时，它会产生下列结果：

Object is being created
Length of line : 6
Object is being deleted
```

### 13.8拷贝构造函数

**拷贝构造函数**是一种特殊的构造函数，它==在创建对象时，是使用同一类中之前创建的对象来初始化新创建的对象==。拷贝构造函数通常用于：

- 通过使用另一个同类型的对象来初始化新创建的对象。
- 复制对象把它作为参数传递给函数。
- 复制对象，并从函数返回这个对象。

==**如果在类中没有定义拷贝构造函数，编译器会自行定义一个。**如果类带有指针变量，并有动态内存分配，则它必须有一个拷贝构造函数。==拷贝构造函数的最常见形式如下：

```c++
classname (const classname &obj) { 
    // 构造函数的主体 
}
```

在这里，**obj** 是一个对象引用，该对象是用于初始化另一个对象的。

```c++
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      int getLength( void );
      Line( int len );             // 简单的构造函数
      Line( const Line &obj);      // 拷贝构造函数
      ~Line();                     // 析构函数
 
   private:
      int *ptr;
};
 
// 成员函数定义，包括构造函数
Line::Line(int len)
{
    cout << "调用构造函数" << endl;
    // 为指针分配内存
    ptr = new int;
    *ptr = len;
}
 
Line::Line(const Line &obj)
{
    cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;
    ptr = new int;
    *ptr = *obj.ptr; // 拷贝值
}
 
Line::~Line(void)
{
    cout << "释放内存" << endl;
    delete ptr;
}
int Line::getLength( void )
{
    return *ptr;
}
 
void display(Line obj)
{
   cout << "line 大小 : " << obj.getLength() <<endl;
}
 
// 程序的主函数
int main( )
{
   Line line(10);
 
   display(line);
 
   return 0;
}
///////////////////////////////////////////////////
当上面的代码被编译和执行时，它会产生下列结果：

调用构造函数
调用拷贝构造函数并为指针 ptr 分配内存
line 大小 : 10
释放内存
释放内存

```

```c++
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      int getLength( void );
      Line( int len );             // 简单的构造函数
      Line( const Line &obj);      // 拷贝构造函数
      ~Line();                     // 析构函数
 
   private:
      int *ptr;
};
 
// 成员函数定义，包括构造函数
Line::Line(int len)
{
    cout << "调用构造函数" << endl;
    // 为指针分配内存
    ptr = new int;
    *ptr = len;
}
 
Line::Line(const Line &obj)
{
    cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;
    ptr = new int;
    *ptr = *obj.ptr; // 拷贝值
}
 
Line::~Line(void)
{
    cout << "释放内存" << endl;
    delete ptr;
}
int Line::getLength( void )
{
    return *ptr;
}
 
void display(Line obj)
{
   cout << "line 大小 : " << obj.getLength() <<endl;
}
 
// 程序的主函数
int main( )
{
   Line line1(10);
 
   Line line2 = line1; // 这里也调用了拷贝构造函数
 
   display(line1);
   display(line2);
 
   return 0;
}
////////////////////////////
当上面的代码被编译和执行时，它会产生下列结果：

调用构造函数
调用拷贝构造函数并为指针 ptr 分配内存
调用拷贝构造函数并为指针 ptr 分配内存
line 大小 : 10
释放内存
调用拷贝构造函数并为指针 ptr 分配内存
line 大小 : 10
释放内存
释放内存
释放内存
```

### 13.9友元函数

1.类的友元函数是==定义在类外部，但有权访问类的所有私有（private）成员和保护（protected）成员==。**尽管友元函数的原型有在类的定义中出现过，但是友元函数并不是成员函数**。

2.友元可以是一个函数，该函数被称为友元函数；友元也可以是一个类，该类被称为友元类，在这种情况下，整个类及其所有成员都是友元。

3.如果要声明函数为一个类的友元，需要在类定义中该函数原型前使用关键字 **friend**，如下所示：

```c++
class Box
{
   double width;
public:
   double length;
   friend void printWidth( Box box );   //要把box类传过去
   void setWidth( double wid );
};
```

4.声明类 ClassTwo 的所有成员函数作为类 ClassOne 的友元，需要在类 ClassOne 的定义中放置如下声明：

```c++
friend class ClassTwo;
```

5.相关函数

```c++
#include <iostream>
 
using namespace std;
 
class Box
{
   double width;
public:
   friend void printWidth( Box box );
   void setWidth( double wid );
};
 
// 成员函数定义
void Box::setWidth( double wid )
{
    width = wid;
}
 
// 请注意：printWidth() 不是任何类的成员函数
void printWidth( Box box )
{
   /* 因为 printWidth() 是 Box 的友元，它可以直接访问该类的任何成员 */
   cout << "Width of box : " << box.width <<endl;
}
 
// 程序的主函数
int main( )
{
   Box box;
 
   // 使用成员函数设置宽度
   box.setWidth(10.0);
   
   // 使用友元函数输出宽度
   printWidth( box );
 
   return 0;
}

当上面的代码被编译和执行时，它会产生下列结果：

Width of box : 10
```

###13.10内联函数

1.C++ **内联函数**是通常与类一起使用。如果一个函数是内联的，==那么在编译时，编译器会把该函数的代码副本放置在每个调用该函数的地方==。

2.对内联函数进行任何修改，都需要重新编译函数的所有客户端，因为编译器需要重新更换一次所有的代码，否则将会继续使用旧的函数。

3.如果想把一个函数定义为内联函数，则需要==在函数名前面放置关键字 **inline**==，在调用函数之前需要对函数进行定义。如果已定义的函数多于一行，编译器会忽略 inline 限定符。

4.==在类定义中的定义的函数都是内联函数==，即使没有使用 **inline** 说明符。

5.下面是一个实例，使用内联函数来返回两个数中的最大值：

```c++
#include <iostream>
 
using namespace std;

inline int Max(int x, int y)
{
   return (x > y)? x : y;
}

// 程序的主函数
int main( )
{
   cout << "Max (20,10): " << Max(20,10) << endl;
   cout << "Max (0,200): " << Max(0,200) << endl;
   cout << "Max (100,1010): " << Max(100,1010) << endl;
   return 0;
}
当上面的代码被编译和执行时，它会产生下列结果：

Max (20,10): 20
Max (0,200): 200
Max (100,1010): 1010
```

###13.11 this指针

1.在 C++ 中，**每一个对象都能通过 this 指针来访问自己的地址**。**this** 指针是所有成员函数的==隐含参数==。因此，==在成员函数内部，它可以用来指向调用对象==。（当我们调用成员函数时，实际上是替某个对象调用它。成员函数通过一个名为 this 的额外隐式参数来访问调用它的那个对象，当我们调用一个成员函数时，用请求该函数的对象地址初始化 this。)

2.友元函数没有 **this** 指针，因为友元不是类的成员。只有成员函数才有 **this** 指针。

3.下面的实例有助于更好地理解 this 指针的概念：

```c++
#include <iostream>
using namespace std;
class Box
{
   public:
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
      }
      double Volume()
      {
         return length * breadth * height;
      }
      int compare(Box box)
      {
         return this->Volume() > box.Volume();
      }
   private:
      double length;     // Length of a box
      double breadth;    // Breadth of a box
      double height;     // Height of a box
};
 
int main(void)
{
   Box Box1(3.3, 1.2, 1.5);    // Declare box1
   Box Box2(8.5, 6.0, 2.0);    // Declare box2
 
   if(Box1.compare(Box2))
   {
      cout << "Box2 is smaller than Box1" <<endl;
   }
   else
   {
      cout << "Box2 is equal to or larger than Box1" <<endl;
   }
   return 0;
}
当上面的代码被编译和执行时，它会产生下列结果：

Constructor called.
Constructor called.
Box2 is equal to or larger than Box1
```



### 13.12指向类的指针

1.一个指向 C++ 类的指针与指向结构的指针类似，访问指向类的指针的成员，需要使用成员访问运算符 **—>**，就像访问指向结构的指针一样。与所有的指针一样，您必须在使用指针之前，对指针进行初始化。

下面的实例有助于更好地理解指向类的指针的概念：

```c++
#include <iostream>
 
using namespace std;

class Box
{
   public:
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
      }
      double Volume()
      {
         return length * breadth * height;
      }
   private:
      double length;     // Length of a box
      double breadth;    // Breadth of a box
      double height;     // Height of a box
};

int main(void)
{
   Box Box1(3.3, 1.2, 1.5);    // Declare box1
   Box Box2(8.5, 6.0, 2.0);    // Declare box2
   Box *ptrBox;                // Declare pointer to a class.

   // 保存第一个对象的地址
   ptrBox = &Box1;

   // 现在尝试使用成员访问运算符来访问成员
   cout << "Volume of Box1: " << ptrBox->Volume() << endl;

   // 保存第二个对象的地址
   ptrBox = &Box2;

   // 现在尝试使用成员访问运算符来访问成员
   cout << "Volume of Box2: " << ptrBox->Volume() << endl;
  
   return 0;
}
当上面的代码被编译和执行时，它会产生下列结果：

Constructor called.
Constructor called.
Volume of Box1: 5.94
Volume of Box2: 102
```

### 13.13  类的静态成员

1.我们可以使用 **static** 关键字来把类成员定义为静态的。当我们声明类的成员为静态时，这意味着==无论创建多少个类的对象，静态成员都只有一个副本==。

2.静态成员在类的所有对象中是==共享的==。如果不存在其他的初始化语句，在创建第一个对象时，所有的静态数据都会被初始化为零。我们不能把静态成员的初始化放置在类的定义中，但是可以在类的外部通过使用范围解析运算符 **::** 来重新声明静态变量从而对它进行初始化，如下面的实例所示。

3.下面的实例有助于更好地理解静态成员数据的概念：

```c++
#include <iostream>
 
using namespace std;
 
class Box
{
   public:
      static int objectCount;
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
         // 每次创建对象时增加 1
         objectCount++;
      }
      double Volume()
      {
         return length * breadth * height;
      }
   private:
      double length;     // 长度
      double breadth;    // 宽度
      double height;     // 高度
};
 
// 初始化类 Box 的静态成员
int Box::objectCount = 0;
 
int main(void)
{
   Box Box1(3.3, 1.2, 1.5);    // 声明 box1
   Box Box2(8.5, 6.0, 2.0);    // 声明 box2
 
   // 输出对象的总数
   cout << "Total objects: " << Box::objectCount << endl;
 
   return 0;
}
当上面的代码被编译和执行时，它会产生下列结果：

Constructor called.
Constructor called.
Total objects: 2 
```

4.如果把函数成员声明为静态的，就可以把函数与类的任何特定对象独立开来。==静态成员函数即使在类对象不存在的情况下也能被调用，**静态函数**只要使用类名加范围解析运算符 **::** 就可以访问==。

5.静态成员函数只能访问静态成员数据、其他静态成员函数和类外部的其他函数。

6.静态成员函数有一个类范围，他们**不能访问类的 this 指针**。您可以使用静态成员函数来判断类的某些对象是否已被创建。

> **静态成员函数与普通成员函数的区别：**
>
> - 静态成员函数没有 this 指针，只能访问静态成员（包括静态成员变量和静态成员函数）。
> - 普通成员函数有 this 指针，可以访问类中的任意成员;

7.下面的实例有助于更好地理解静态成员函数的概念：

```c++
#include <iostream>
 
using namespace std;
 
class Box
{
   public:
      static int objectCount;
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
         // 每次创建对象时增加 1
         objectCount++;
      }
      double Volume()
      {
         return length * breadth * height;
      }
      static int getCount()
      {
         return objectCount;
      }
   private:
      double length;     // 长度
      double breadth;    // 宽度
      double height;     // 高度
};
 
// 初始化类 Box 的静态成员
int Box::objectCount = 0;
 
int main(void)
{
  
   // 在创建对象之前输出对象的总数
   cout << "Inital Stage Count: " << Box::getCount() << endl;
 
   Box Box1(3.3, 1.2, 1.5);    // 声明 box1
   Box Box2(8.5, 6.0, 2.0);    // 声明 box2
 
   // 在创建对象之后输出对象的总数
   cout << "Final Stage Count: " << Box::getCount() << endl;
 
   return 0;
}
//////////////////////////////////////////////////
当上面的代码被编译和执行时，它会产生下列结果：

Inital Stage Count: 0
Constructor called.
Constructor called.
Final Stage Count: 2
```

##14.继承

1.面向对象程序设计中最重要的一个概念是继承。继承允许我们==依据另一个类来定义一个类==，这使得创建和维护一个应用程序变得更容易。这样做，也达到了重用代码功能和提高执行效率的效果。

2.当创建一个类时，您不需要重新编写新的数据成员和成员函数，只需指定新建的类继承了一个已有的类的成员即可。这个已有的类称为**基类**，新建的类称为**派生类**。

3.一个派生类继承了所有的基类方法，但下列情况除外：

- 基类的构造函数、析构函数和拷贝构造函数。
- 基类的重载运算符。
- 基类的友元函数。

4.继承代表了 **is a** 关系。例如，哺乳动物是动物，狗是哺乳动物，因此，狗是动物，等等。

````c++
// 基类
class Animal {
    // eat() 函数
    // sleep() 函数
};


//派生类
class Dog : public Animal {
    // bark() 函数
};
````

5.一个类可以派生自多个类，这意味着，它可以从多个基类继承数据和函数。定义一个派生类，我们使用一个类派生列表来指定基类。类派生列表以一个或多个基类命名，形式如下：

```c++
class derived-class: access-specifier base-class
```

其中，访问修饰符 access-specifier 是 **public、protected** 或 **private** 其中的一个，base-class 是之前定义过的某个类的名称。如果未使用访问修饰符 access-specifier，则默认为 private。

```c++
#include <iostream>
 
using namespace std;
 
// 基类
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};
 
// 派生类
class Rectangle: public Shape
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};
 
int main(void)
{
   Rectangle Rect;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
 
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;
 
   return 0;
}

当上面的代码被编译和执行时，它会产生下列结果：

Total area: 35

```

### 14.1多继承

多继承即一个子类可以有多个父类，它继承了多个父类的特性。

C++ 类可以从多个类继承成员，语法如下：

```c++
class <派生类名>:<继承方式1><基类名1>,<继承方式2><基类名2>,…
{
<派生类类体>
};
```

其中，访问修饰符继承方式是 **public、protected** 或 **private** 其中的一个，用来修饰每个基类，==各个基类之间用逗号分隔==，如上所示。现在让我们一起看看下面的实例：

```c++
#include <iostream>
using namespace std;
 // 基类 Shape
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};
 
// 基类 PaintCost
class PaintCost 
{
   public:
      int getCost(int area)
      {
         return area * 70;
      }
};
 
// 派生类
class Rectangle: public Shape, public PaintCost
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};
 
int main(void)
{
   Rectangle Rect;
   int area;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
 
   area = Rect.getArea();
   
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;
 
   // 输出总花费
   cout << "Total paint cost: $" << Rect.getCost(area) << endl;
 
   return 0;
}

/////////////////////////////////////////////////
/////////////////////////////////////////////////
当上面的代码被编译和执行时，它会产生下列结果：

Total area: 35
Total paint cost: $2450
```

###14.2

## 15.重载运算符和重载函数

C++ 允许在同一作用域中的某个**函数**和**运算符**指定多个定义，分别称为**函数重载**和**运算符重载**。

重载声明是指一个与之前已经在该作用域内声明过的函数或方法具有相同名称的声明，但是它们的参数列表和定义（实现）不相同。

当您调用一个**重载函数**或**重载运算符**时，编译器通过把您所使用的参数类型与定义中的参数类型进行比较，决定选用最合适的定义。选择最合适的重载函数或重载运算符的过程，称为**重载决策**。

### 15.1C++ 中的函数重载

 **在同一个作用域内，可以声明几个功能类似的同名函数，但是这些同名函数的==形式参数==（指参数的个数、类型或者顺序）==必须不同==**。您不能仅通过返回类型的不同来重载函数。

下面的实例中，同名函数 **print()** 被用于输出不同的数据类型：

```c++
#include <iostream>
using namespace std;
class printData
{
   public:
      void print(int i) {
        cout << "整数为: " << i << endl;
      }
 
      void print(double  f) {
        cout << "浮点数为: " << f << endl;
      }
 
      void print(char c[]) {
        cout << "字符串为: " << c << endl;
      }
};

int main(void)
{
   printData pd;
 
   // 输出整数
   pd.print(5);
   // 输出浮点数
   pd.print(500.263);
   // 输出字符串
   char c[] = "Hello C++";
   pd.print(c);
   return 0;
}

/////////
当上面的代码被编译和执行时，它会产生下列结果：

整数为: 5
浮点数为: 500.263
字符串为: Hello C++
```

### 15.2C++ 中的运算符重载

您可以重定义或重载大部分 C++ 内置的运算符。这样，您就能使用自定义类型的运算符。

重载的运算符是==带有特殊名称的函数==，函数名是由**关键字 operator 和其后要重载的运算符**符号构成的。与其他函数一样，重载运算符有一个返回类型和一个参数列表。

```c++
Box operator+(const Box&);
```

声明加法运算符用于把两个 Box 对象相加，返回最终的 Box 对象。大多数的重载运算符可被定义为普通的非成员函数或者被定义为类成员函数。如果我们定义上面的函数为类的非成员函数，那么我们需要为每次操作传递两个参数，如下所示：

```c++
Box operator+(const Box&, const Box&);
```

下面的实例使用成员函数演示了运算符重载的概念。在这里，对象作为参数进行传递，对象的属性使用 **this** 运算

```c++
#include <iostream>
using namespace std;
 class Box
{
   public:
 
      double getVolume(void)
      {
         return length * breadth * height;
      }
      void setLength( double len )
      {
          length = len;
      }
 
      void setBreadth( double bre )
      {
          breadth = bre;
      }
 
      void setHeight( double hei )
      {
          height = hei;
      }
      // 重载 + 运算符，用于把两个 Box 对象相加
      Box operator+(const Box& b)
      {
         Box box;
         box.length = this->length + b.length;
         box.breadth = this->breadth + b.breadth;
         box.height = this->height + b.height;
         return box;
      }
   private:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
};
// 程序的主函数
int main( )
{
   Box Box1;                // 声明 Box1，类型为 Box
   Box Box2;                // 声明 Box2，类型为 Box
   Box Box3;                // 声明 Box3，类型为 Box
   double volume = 0.0;     // 把体积存储在该变量中
 
   // Box1 详述
   Box1.setLength(6.0); 
   Box1.setBreadth(7.0); 
   Box1.setHeight(5.0);
 
   // Box2 详述
   Box2.setLength(12.0); 
   Box2.setBreadth(13.0); 
   Box2.setHeight(10.0);
 
   // Box1 的体积
   volume = Box1.getVolume();
   cout << "Volume of Box1 : " << volume <<endl;
 
   // Box2 的体积
   volume = Box2.getVolume();
   cout << "Volume of Box2 : " << volume <<endl;
 
   // 把两个对象相加，得到 Box3
   Box3 = Box1 + Box2;
 
   // Box3 的体积
   volume = Box3.getVolume();
   cout << "Volume of Box3 : " << volume <<endl;
 
   return 0;
}
```

### 15.3可重载运算符/不可重载运算符

下面是可重载的运算符列表：

| 双目算术运算符 | + (加)，-(减)，*(乘)，/(除)，% (取模)                        |
| -------------- | ------------------------------------------------------------ |
| 关系运算符     | ==(等于)，!= (不等于)，< (小于)，> (大于)，<=(小于等于)，>=(大于等于) |
| 逻辑运算符     | \|\|(逻辑或)，&&(逻辑与)，!(逻辑非)                          |
| 单目运算符     | + (正)，-(负)，*(指针)，&(取地址)                            |
| 自增自减运算符 | ++(自增)，--(自减)                                           |
| 位运算符       | \| (按位或)，& (按位与)，~(按位取反)，^(按位异或),，<< (左移)，>>(右移) |
| 赋值运算符     | =, +=, -=, *=, /= , % = , &=, \|=, ^=, <<=, >>=              |
| 空间申请与释放 | new, delete, new[ ] , delete[]                               |
| 其他运算符     | **()**(函数调用)，**->**(成员访问)，**,**(逗号)，**[]**(下标) |

下面是不可重载的运算符列表：

- **.**：成员访问运算符
- **.\***, **->\***：成员指针访问运算符
- **::**：域运算符
- **sizeof**：长度运算符
- **?:**：条件运算符
- **#**： 预处理符号

###15.4 运算符重载实例

下面提供了各种运算符重载的实例，帮助您更好地理解重载的概念。

| 序号 | 运算符和实例                                                 |
| :--- | :----------------------------------------------------------- |
| 1    | [一元运算符重载](https://www.runoob.com/cplusplus/unary-operators-overloading.html) |
| 2    | [二元运算符重载](https://www.runoob.com/cplusplus/binary-operators-overloading.html) |
| 3    | [关系运算符重载](https://www.runoob.com/cplusplus/relational-operators-overloading.html) |
| 4    | [输入/输出运算符重载](https://www.runoob.com/cplusplus/input-output-operators-overloading.html) |
| 5    | [++ 和 -- 运算符重载](https://www.runoob.com/cplusplus/increment-decrement-operators-overloading.html) |
| 6    | [赋值运算符重载](https://www.runoob.com/cplusplus/assignment-operators-overloading.html) |
| 7    | [函数调用运算符 () 重载](https://www.runoob.com/cplusplus/function-call-operator-overloading.html) |
| 8    | [下标运算符 [\] 重载](https://www.runoob.com/cplusplus/subscripting-operator-overloading.html) |
| 9    | [类成员访问运算符 -> 重载](https://www.runoob.com/cplusplus/class-member-access-operator-overloading.html) |

## 16.引用

引用变量是一个别名，也就是说，它是某个已存在变量的另一个名字。一旦把引用初始化为某个变量，就可以使用该引用名称或变量名称来指向变量。

### 16.1引用 和 指针 的区别

引用很容易与指针混淆，它们之间有三个主要的不同：

- ==不存在空引用==。引用必须连接到一块合法的内存。
- 一旦引用被初始化为一个对象，就==不能被指向到另一个对象==。指针可以在任何时候指向到另一个对象。
- 引用==必须在创建时被初始化==。指针可以在任何时间被初始化。

### 16.2C++ 中创建引用

试想变量名称是变量附属在内存位置中的标签，您可以把引用当成是变量附属在内存位置中的第二个标签。因此，您可以通过原始变量名称或引用来访问变量的内容。例如：

```c++
int i = 17;
```

我们可以为 i 声明引用变量，如下所示：

```c++
int&  r = i;
double& s = d;
```

在这些声明中，& 读作**引用**。因此，第一个声明可以读作 ”r“ 是一个初始化为 i 的整型引用"，第二个声明可以读作 "s 是一个初始化为 d 的 double 型引用"。下面的实例使用了 int 和 double 引用：

```c++
#include <iostream> 
using namespace std; 
int main () {  
    // 声明简单的变量  
    int    i; 
    double d;
    // 声明引用变量   
    int&    r = i;  
    double& s = d;   
    i = 5; 
    cout << "Value of i : " << i << endl;  
    cout << "Value of i reference : " << r  << endl;
    d = 11.7; 
    cout << "Value of d : " << d << endl;   
    cout << "Value of d reference : " << s  << endl; 
    return 0; 
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```c++
Value of i : 5
Value of i reference : 5
Value of d : 11.7
Value of d reference : 11.7
```

引用通常用于函数参数列表和函数返回值。下面列出了 C++ 程序员必须清楚的两个与 C++ 引用相关的重要概念：

| 概念                                                         | 描述                                                     |
| :----------------------------------------------------------- | :------------------------------------------------------- |
| [把引用作为参数](https://www.runoob.com/cplusplus/passing-parameters-by-references.html) | C++ 支持把引用作为参数传给函数，这比传一般的参数更安全。 |
| [把引用作为返回值](https://www.runoob.com/cplusplus/returning-values-by-reference.html) | 可以从 C++ 函数中返回引用，就像返回其他数据类型一样      |

## 17.多态

**多态**按字面的意思就是多种形态。当类之间存在层次结构，并且类之间是通过继承关联时，就会用到多态。

下面的实例中，基类 Shape 被派生为两个类，如下所示：

```c++
#include <iostream> 
using namespace std;

class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      int area()
      {
         cout << "Parent class area :" <<endl;
         return 0;
      }
};
class Rectangle: public Shape{
   public:
      Rectangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Rectangle class area :" <<endl;
         return (width * height); 
      }
};
class Triangle: public Shape{
   public:
      Triangle( int a=0, int b=0):Shape(a, b) { }//构造函数无法继承，需重新构造
      int area ()
      { 
         cout << "Triangle class area :" <<endl;
         return (width * height / 2); 
      }
};
// 程序的主函数
int main( )
{
   Shape *shape;
   Rectangle rec(10,7);
   Triangle  tri(10,5);
 
   // 存储矩形的地址
   shape = &rec;
   // 调用矩形的求面积函数 area
   shape->area();
 
   // 存储三角形的地址
   shape = &tri;
   // 调用三角形的求面积函数 area
   shape->area();
   return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```c++
Parent class area :
Parent class area :
```

导致错误输出的原因是，调用函数 area() 被编译器设置为基类中的版本，这就是所谓的**静态多态**，或**静态链接** - 函数调用在程序执行前就准备好了。有时候这也被称为**早绑定**，因为 area() 函数在程序编译期间就已经设置好了。

但现在，让我们对程序稍作修改，在 Shape 类中，area() 的声明前放置关键字 **virtual**，如下所示：

```c++
class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      virtual int area()
      {
         cout << "Parent class area :" <<endl;
         return 0;
      }
};
///////////////////////////////////////////////

修改后，当编译和执行前面的实例代码时，它会产生以下结果：

Rectangle class area :
Triangle class area :
```

纯虚函数

您可能想要在基类中定义虚函数，以便在派生类中重新定义该函数更好地适用于对象，但是您在基类中又不能对虚函数给出有意义的实现，这个时候就会用到纯虚函数。

我们可以把基类中的虚函数 area() 改写如下

```c++
class Shape {   
    protected:  
         int width, height;  
    public:  
          Shape( int a=0, int b=0)
          {    
              width = a;  
              height = b;     
          }   
    // pure virtual function    
    virtual int area() = 0; 
};
```

= 0 告诉编译器，函数没有主体，上面的虚函数是**纯虚函数**。

## 18.动态分配内存new

```c++
int *ptr = new int; // ptr-栈区，int 在堆区分配了一块int型的空间
ptr ++; // 这个操作很危险，使得上面的Int的型空间没人管了，内存泄漏
delete ptr;   //释放内存，注意，不要释放已释放的内存，也不要释放一般变量的内存

////////////////////////////////////////////////

int *intArray = new int[10]; // new 返回第一个元素的地址
delete [] intArray;
```

## 19.数据抽象

1.数据抽象是指，==只向外界提供关键信息，并隐藏其后台的实现细节，即只表现必要的信息而不呈现细节==。

2.数据抽象是一种依赖于接口和实现分离的编程（设计）技术。

3.就 C++ 编程而言，C++==类==为**数据抽象**提供了可能。它们向外界提供了大量用于操作对象数据的公共方法，也就是说，外界实际上并不清楚类的内部实现。

### 19.1访问标签强制抽象

在 C++ 中，我们使用访问标签来定义类的抽象接口。一个类可以包含零个或多个访问标签：

- 使用公共标签定义的成员都可以访问该程序的所有部分。一个类型的数据抽象视图是由它的公共成员来定义的。
- 使用私有标签定义的成员无法访问到使用类的代码。私有部分对使用类型的代码隐藏了实现细节。

访问标签出现的频率没有限制。每个访问标签指定了紧随其后的成员定义的访问级别。指定的访问级别会一直有效，直到遇到下一个访问标签或者遇到类主体的关闭右括号为止。

### 19.2数据抽象的好处

数据抽象有两个重要的优势：

- **类的内部受到保护**，不会因无意的用户级错误导致对象状态受损。
- 类实现可能随着时间的推移而发生变化，以便应对不断变化的需求，或者应对那些要求不改变用户级代码的错误报告。

如果只在类的私有部分定义数据成员，编写该类的作者就可以随意更改数据。如果实现发生改变，则只需要检查类的代码，看看这个改变会导致哪些影响。如果数据是公有的，则任何直接访问旧表示形式的数据成员的函数都可能受到影响。

###19.3 数据抽象的实例

C++ 程序中，任何带有公有和私有成员的类都可以作为数据抽象的实例。请看下面的实例：

```c++
#include <iostream>
using namespace std;
 
class Adder{
   public:
      // 构造函数
      Adder(int i = 0)
      {
        total = i;
      }
      // 对外的接口
      void addNum(int number)
      {
          total += number;
      }
      // 对外的接口
      int getTotal()
      {
          return total;
      };
   private:
      // 对外隐藏的数据
      int total;
};
int main( )
{
   Adder a;
   
   a.addNum(10);
   a.addNum(20);
   a.addNum(30);
 
   cout << "Total " << a.getTotal() <<endl;
   return 0;
}
```

上面的类把数字相加，并返回总和。公有成员 **addNum** 和 **getTotal** 是对外的接口，用户需要知道它们以便使用类。私有成员 **total** 是用户不需要了解的，但又是类能正常工作所必需的。

###19.4 设计策略

抽象把代码分离为接口和实现。所以在设计组件时，必须保持==接口独立于实现==，这样，如果改变底层实现，接口也将保持不变。

在这种情况下，不管任何程序使用接口，接口都不会受到影响，只需要将最新的实现重新编译即可。

##20.数据封装

1.**数据封装**是一种把数据和操作数据的函数捆绑在一起的机制，**数据抽象**是一种仅向用户暴露接口而把具体的实现细节隐藏起来的机制。

2.C++ 通过创建**类**来支持封装和数据隐藏（public、protected、private）。我们已经知道，类包含私有成员（private）、保护成员（protected）和公有成员（public）成员。默认情况下，在类中定义的所有项目都是私有的。例如：

```c++
class Box
{
   public:
      double getVolume(void)
      {
         return length * breadth * height;
      }
   private:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
};
```

变量 length、breadth 和 height 都是私有的（private）。这意味着它们只能被 Box 类中的其他成员访问，而不能被程序中其他部分访问。这是实现封装的一种方式。

3.为了使类中的成员变成公有的（即，程序中的其他部分也能访问），必须在这些成员前使用 **public** 关键字进行声明。所有定义在 public 标识符后边的变量或函数可以被程序中所有其他的函数访问。

4.把一个类定义为另一个类的友元类，会暴露实现细节，从而降低了封装性。理想的做法是尽可能地对外隐藏每个类的实现细节。

5.C++ 程序中，任何带有公有和私有成员的类都可以作为数据封装和数据抽象的实例。请看下面的实例：

```c++
#include <iostream>
using namespace std;
 
class Adder{
   public:
      // 构造函数
      Adder(int i = 0)
      {
        total = i;
      }
      // 对外的接口
      void addNum(int number)
      {
          total += number;
      }
      // 对外的接口
      int getTotal()
      {
          return total;
      };
   private:
      // 对外隐藏的数据
      int total;
};
int main( )
{
   Adder a;
   a.addNum(10);
   a.addNum(20);
   a.addNum(30);
 
   cout << "Total " << a.getTotal() <<endl;
   return 0;
}
```

##21.接口（抽象类）

1.C++ 接口是使用**抽象类**来实现的，抽象类与数据抽象互不混淆，数据抽象是一个把实现细节与相关的数据分离开的概念。

2.**如果类中至少有一个函数被声明为纯虚函数，则这个类就是抽象类**。纯虚函数是通过在声明中使用 "= 0" 来指定的，如下所示：

```c++
class Box {
    public:    
          // 纯虚函数 
          virtual double getVolume() = 0; 
    private:
           double length;      // 长度 
           double breadth;     // 宽度    
           double height;      // 高度
};
```

设计**抽象类**（通常称为 ABC）的目的，是为了给其他类**提供一个可以继承的适当的基类**。抽象类不能被用于实例化对象，它只能作为**接口**使用。**如果试图实例化一个抽象类的对象，会导致编译错误**。因此，如果一个 ABC 的子类需要被实例化，则必须实现每个虚函数，这也意味着 C++ 支持使用 ABC 声明接口。如果没有在派生类中重写纯虚函数，就尝试实例化该类的对象，会导致编译错误。

3.可用于实例化对象的类被称为**具体类**。

4.请看下面的实例，基类 Shape 提供了一个接口 **getArea()**，在两个派生类 Rectangle 和 Triangle 中分别实现了 **getArea()**：

```c++
#include <iostream>
 
using namespace std;
 
// 基类
class Shape 
{
public:
   // 提供接口框架的纯虚函数
   virtual int getArea() = 0;
   void setWidth(int w)
   {
      width = w;
   }
   void setHeight(int h)
   {
      height = h;
   }
protected:
   int width;
   int height;
};
 
// 派生类
class Rectangle: public Shape
{
public:
   int getArea()
   { 
      return (width * height); 
   }
};
class Triangle: public Shape
{
public:
   int getArea()
   { 
      return (width * height)/2; 
   }
};
 
int main(void)
{
   Rectangle Rect;
   Triangle  Tri;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
   // 输出对象的面积
   cout << "Total Rectangle area: " << Rect.getArea() << endl;
 
   Tri.setWidth(5);
   Tri.setHeight(7);
   // 输出对象的面积
   cout << "Total Triangle area: " << Tri.getArea() << endl; 
 
   return 0;
}

当上面的代码被编译和执行时，它会产生下列结果：

Total Rectangle area: 35
Total Triangle area: 17
```

面向对象的系统可能会**使用一个抽象基类为所有的外部应用程序提供一个适当的、通用的、标准化的接口**。然后，**派生类通过继承抽象基类，就把所有类似的操作都继承下来**。外部应用程序提供的功能（即公有函数）在抽象基类中是以纯虚函数的形式存在的。这些纯虚函数在相应的派生类中被实现。这个架构也使得新的应用程序可以很容易地被添加到系统中，即使是在系统被定义之后依然可以如此。

## 22.文件和流

需要用到 C++ 中另一个标准库 **fstream**，它定义了三个新的数据类型：

| 数据类型 | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| ofstream | 该数据类型表示输出文件流，用于创建文件并向文件写入信息。     |
| ifstream | 该数据类型表示输入文件流，用于从文件读取信息。               |
| fstream  | 该数据类型通常表示文件流，且同时具有 ofstream 和 ifstream 两种功能，这意味着它可以创建文件，向文件写入信息，从文件读取信息。 |

要在 C++ 中进行文件处理，必须在 C++ 源代码文件中包含头文件 <iostream\>

 和 <fstream\>。

### 22.1打开文件

在从文件读取信息或者向文件写入信息之前，必须先打开文件。**ofstream** 和 **fstream** 对象都可以用来打开文件进行写操作，如果只需要打开文件进行读操作，则使用 **ifstream** 对象。

下面是 open() 函数的标准语法，open() 函数是 fstream、ifstream 和 ofstream 对象的一个成员。

```c++
void open(const char *filename, ios::openmode mode);
```

在这里，**open()** 成员函数的第一参数指定要打开的文件的名称和位置，第二个参数定义文件被打开的模式。

| 模式标志   | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| ios::app   | 追加模式。所有写入都追加到文件末尾。                         |
| ios::ate   | 文件打开后定位到文件末尾。                                   |
| ios::in    | 打开文件用于读取。                                           |
| ios::out   | 打开文件用于写入。                                           |
| ios::trunc | 如果该文件已经存在，其内容将在打开文件之前被截断，即把文件长度设为 0。 |

您可以把以上两种或两种以上的模式结合使用。例如，如果您想要以写入模式打开文件，并希望截断文件，以防文件已存在，那么您可以使用下面的语法：

```c++
ofstream outfile;
outfile.open("file.dat", ios::out | ios::trunc )
```

类似地，您如果想要打开一个文件用于读写，可以使用下面的语法：

```c++
ifstream  afile;
afile.open("file.dat", ios::out | ios::in );
```

### 22.2关闭文件

当 C++ 程序终止时，它会自动关闭刷新所有流，释放所有分配的内存，并关闭所有打开的文件。但程序员应该养成一个好习惯，在程序终止前关闭所有打开的文件。

下面是 close() 函数的标准语法，close() 函数是 fstream、ifstream 和 ofstream 对象的一个成员。

```c++
void close();
```

###22.3 写入文件

在 C++ 编程中，我们使用流插入运算符（ << ）向文件写入信息，就像使用该运算符输出信息到屏幕上一样。唯一不同的是，在这里您使用的是 **ofstream** 或 **fstream** 对象，而不是 **cout** 对象。

### 22.4读取 & 写入实例

下面的 C++ 程序以读写模式打开一个文件。在向文件 afile.dat 写入用户输入的信息之后，程序从文件读取信息，并将其输出到屏幕上：

```c++
#include <fstream>
#include <iostream>
using namespace std;
 
int main ()
{
    
   char data[100];
 
   // 以写模式打开文件
   ofstream outfile;
   outfile.open("afile.dat");
      
   cout << "Writing to the file" << endl;
   cout << "Enter your name: "; 
   cin.getline(data, 100);
 
   // 向文件写入用户输入的数据
   outfile << data << endl;
 
   cout << "Enter your age: "; 
   cin >> data;
   cin.ignore();
   
   // 再次向文件写入用户输入的数据
   outfile << data << endl;
 
   // 关闭打开的文件
   outfile.close();
 
   // 以读模式打开文件
   ifstream infile; 
   infile.open("afile.dat"); 
    
   cout << "Reading from the file" << endl; 
   infile >> data; 
    
   // 在屏幕上写入数据
   cout << data << endl;
   
   // 再次从文件读取数据，并显示它
   infile >> data; 
   cout << data << endl; 
 
   // 关闭打开的文件
   infile.close();
 
   return 0;
}
当上面的代码被编译和执行时，它会产生下列输入和输出：

$./a.out
Writing to the file
Enter your name: Zara
Enter your age: 9
Reading from the file
Zara
9
```

上面的实例中使用了 cin 对象的附加函数，比如 getline()函数从外部读取一行，ignore() 函数会忽略掉之前读语句留下的多余字符。

### 22.5文件位置指针

1.**istream** 和 **ostream** 都提供了用于重新定位文件位置指针的成员函数。这些成员函数包括关于 istream 的 **seekg**（"seek get"）和关于 ostream 的 **seekp**（"seek put"）。

2.seekg 和 seekp 的参数通常是一个长整型。第二个参数可以用于指定查找方向。查找方向可以是 **ios::beg**（默认的，从流的开头开始定位），也可以是 **ios::cur**（从流的当前位置开始定位），也可以是 **ios::end**（从流的末尾开始定位）。

3.文件位置指针是一个整数值，指定了从文件的起始位置到指针所在位置的字节数。下面是关于定位 "get" 文件位置指针的实例：

```c++
// 定位到 fileObject 的第 n 个字节（假设是 ios::beg）
fileObject.seekg( n );
 
// 把文件的读指针从 fileObject 当前位置向后移 n 个字节
fileObject.seekg( n, ios::cur );
 
// 把文件的读指针从 fileObject 末尾往回移 n 个字节
fileObject.seekg( n, ios::end );
 
// 定位到 fileObject 的末尾
fileObject.seekg( 0, ios::end );
```

## 23.异常处理

异常是程序在执行期间产生的问题。C++ 异常是指在==程序运行时发生的特殊情况==，比如尝试除以零的操作。

异常提供了一种转移程序控制权的方式。C++ 异常处理涉及到三个关键字：**try、catch、throw**。

- **throw:** 当问题出现时，程序会抛一个异常。这是通过使用 **throw** 关键字来完成的。
- **catch:** 在您想要处理问题的地方，通过异常处理程序捕获异常。**catch** 关键字用于捕获异常。
- **try:** **try** 块中的代码标识将被激活的特定异常。它后面通常跟着一个或多个 catch 块。

如果有一个块抛出一个异常，捕获异常的方法会使用 **try** 和 **catch** 关键字。try 块中放置可能抛出异常的代码，try 块中的代码被称为保护代码。使用 try/catch 语句的语法如下所示：

```c++
try {   
    // 保护代码 
}catch( ExceptionName e1 ) {
    // catch 块 
}catch( ExceptionName e2 ) {
    // catch 块 
}catch( ExceptionName eN ) { 
    // catch 块 
}
```



如果 **try** 块在不同的情境下会抛出不同的异常，这个时候可以尝试罗列多个 **catch** 语句，用于捕获不同类型的异常。

### 23.1抛出异常

您可以使用 **throw** 语句在代码块中的任何地方抛出异常。throw 语句的操作数可以是任意的表达式，表达式的结果的类型决定了抛出的异常的类型。

以下是尝试除以零时抛出异常的实例：

```c++
double division(int a, int b) {
    if( b == 0 ) 
    { 
        throw "Division by zero condition!";
    } 
    return (a/b);
}
```

### 23.2捕获异常

**catch** 块跟在 **try** 块后面，用于捕获异常。您可以指定想要捕捉的异常类型，这是由 catch 关键字后的括号内的异常声明决定的。

```c++
try {
    // 保护代码 
}catch( ExceptionName e ) {
    // 处理 ExceptionName 异常的代码 
}
```

上面的代码会捕获一个类型为 **ExceptionName** 的异常。如果您想让 catch 块能够处理 try 块抛出的任何类型的异常，则必须在异常声明的括号内使用省略号 ...，如下所示：

```c++
try {   
    // 保护代码 
}catch(...) {
    // 能处理任何异常的代码 
}
```

下面是一个实例，抛出一个除以零的异常，并在 catch 块中捕获该异常。

```c++
#include <iostream>
using namespace std;
 
double division(int a, int b)
{
   if( b == 0 )
   {
      throw "Division by zero condition!";
   }
   return (a/b);
}
 
int main ()
{
   int x = 50;
   int y = 0;
   double z = 0;
 
   try {
     z = division(x, y);
     cout << z << endl;
   }catch (const char* msg) {
     cerr << msg << endl;
   }
 
   return 0;
}
```

由于我们抛出了一个类型为 **const char\*** 的异常，因此，当捕获该异常时，我们必须在 catch 块中使用 const char*。当上面的代码被编译和执行时，它会产生下列结果：

```c++
Division by zero condition!
```

###23.3标准的异常

C++ 提供了一系列标准的异常，定义在 **<exception\>** 中，我们可以在程序中使用这些标准的异常。它们是以父子类层次结构组织起来的，如下所示：

![C++ 异常的层次结构](https://www.runoob.com/wp-content/uploads/2015/05/exceptions_in_cpp.png)

下表是对上面层次结构中出现的每个异常的说明：

| 异常                   | 描述                                                         |
| :--------------------- | :----------------------------------------------------------- |
| **std::exception**     | 该异常是所有标准 C++ 异常的父类。                            |
| std::bad_alloc         | 该异常可以通过 **new** 抛出。                                |
| std::bad_cast          | 该异常可以通过 **dynamic_cast** 抛出。                       |
| std::bad_exception     | 这在处理 C++ 程序中无法预期的异常时非常有用。                |
| std::bad_typeid        | 该异常可以通过 **typeid** 抛出。                             |
| **std::logic_error**   | 理论上可以通过读取代码来检测到的异常。                       |
| std::domain_error      | 当使用了一个无效的数学域时，会抛出该异常。                   |
| std::invalid_argument  | 当使用了无效的参数时，会抛出该异常。                         |
| std::length_error      | 当创建了太长的 std::string 时，会抛出该异常。                |
| std::out_of_range      | 该异常可以通过方法抛出，例如 std::vector 和 std::bitset<>::operator[]()。 |
| **std::runtime_error** | 理论上不可以通过读取代码来检测到的异常。                     |
| std::overflow_error    | 当发生数学上溢时，会抛出该异常。                             |
| std::range_error       | 当尝试存储超出范围的值时，会抛出该异常。                     |
| std::underflow_error   | 当发生数学下溢时，会抛出该异常。                             |

## 24.vector初始化

### 24.1说明

vector是==向量类型==，可容纳许多类型的数据，也被称为容器。（可理解为动态数组）

### 24.2初始化

```c++
//定义具有10个整型元素的向量（尖括号为元素类型名，它可以是任何合法的数据类型），不具有初值，其值不确定
vector<int>a(10);

//定义具有10个整型元素的向量，且给出的每个元素初值为1
vector<int>a(10,1);

//用向量b给向量a赋值，a的值完全等价于b的值
vector<int>a(b);

//将向量b中从0-2（共三个）的元素赋值给a，a的类型为int型
vector<int>a(b.begin(),b.begin+3);

 //从数组中获得初值
int b[7]={1,2,3,4,5,6,7};
vector<int> a(b,b+7）;
```

### 24.3内置函数

```c++
#include<vector>
vector<int> a,b;
//b为向量，将b的0-2个元素赋值给向量a
a.assign(b.begin(),b.begin()+3);
//a含有4个值为2的元素
a.assign(4,2);
//返回a的最后一个元素
a.back();
//返回a的第一个元素
a.front();
//返回a的第i元素,当且仅当a存在
a[i];
//清空a中的元素
a.clear();
//判断a是否为空，空则返回true，非空则返回false
a.empty();
//删除a向量的最后一个元素
a.pop_back();
//删除a中第一个（从第0个算起）到第二个元素，也就是说删除的元素从a.begin()+1算起（包括它）一直到a.begin()+3（不包括它）结束
a.erase(a.begin()+1,a.begin()+3);
//在a的最后一个向量后插入一个元素，其值为5
a.push_back(5);
//在a的第一个元素（从第0个算起）位置插入数值5,
a.insert(a.begin()+1,5);
//在a的第一个元素（从第0个算起）位置插入3个数，其值都为5
a.insert(a.begin()+1,3,5);
//b为数组，在a的第一个元素（从第0个元素算起）的位置插入b的第三个元素到第5个元素（不包括b+6）
a.insert(a.begin()+1,b+3,b+6);
//返回a中元素的个数
a.size();
//返回a在内存中总共可以容纳的元素个数
a.capacity();
//将a的现有元素个数调整至10个，多则删，少则补，其值随机
a.resize(10);
//将a的现有元素个数调整至10个，多则删，少则补，其值为2
a.resize(10,2);
//将a的容量扩充至100，
a.reserve(100);
//b为向量，将a中的元素和b中的元素整体交换
a.swap(b);
//b为向量，向量的比较操作还有 != >= > <= <
a==b;
```

###24.4顺序访问vector的方式

```c++
//.向向量a中添加元素
vector<int>a;
for(int i=0;i<10;++i){a.push_back(i);}
```

```c++
//从数组中选择元素向向量中添加
int a[6]={1,2,3,4,5,6};
vector<int> b;
for(int i=0;i<=4;++i){b.push_back(a[i]);}
```

```c++
//从现有向量中选择元素向向量中添加
int a[6]={1,2,3,4,5,6};
vector<int>b;
vector<int>c(a,a+4);
for(vector<int>::iterator it=c.begin();it<c.end();++it)
{
	b.push_back(*it);
}
```

```c++
//从文件中读取元素向向量中添加
ifstream in("data.txt");
vector<int>a;
for(int i;in>>i){a.push_back(i);}
```

```c++
//常见错误赋值方式
vector<int>a;
for(int i=0;i<10;++i){a[i]=i;}//下标只能用来获取已经存在的元素    
```

### 24.5从向量中读取元素

```c++
//通过下标方式获取
int a[6]={1,2,3,4,5,6};
vector<int>b(a,a+4);
for(int i=0;i<=b.size()-1;++i){cout<<b[i]<<endl;}
```

```c++
//通过迭代器方式读取
 int a[6]={1,2,3,4,5,6};
 vector<int>b(a,a+4);
 for(vector<int>::iterator it=b.begin();it!=b.end();it++){cout<<*it<<"  ";}
```

### 24.6几个常用的算法

```c++
 #include<algorithm>
 //对a中的从a.begin()（包括它）到a.end()（不包括它）的元素进行从小到大排列
 sort(a.begin(),a.end());
 //对a中的从a.begin()（包括它）到a.end()（不包括它）的元素倒置，但不排列，如a中元素为1,3,2,4,倒置后为4,2,3,1
 reverse(a.begin(),a.end());
  //把a中的从a.begin()（包括它）到a.end()（不包括它）的元素复制到b中，从b.begin()+1的位置（包括它）开始复制，覆盖掉原有元素
 copy(a.begin(),a.end(),b.begin()+1);
 //在a中的从a.begin()（包括它）到a.end()（不包括它）的元素中查找10，若存在返回其在向量中的位置
  find(a.begin(),a.end(),10);
```



## 25.C++Lambda表达式详解

### 25.1概述

 lambda表达式（也称为lambda函数）**是在调用或作为函数参数传递的位置处定义匿名函数对象的便捷方法**。通常，lambda用于封装传递给算法或异步方法的几行代码 。

### 25.2例子

```c++	
#include <algorithm>
#include <cmath>

void abssort(float* x, unsigned n) {
    std::sort(x, x + n,
        // Lambda expression begins
        [](float a, float b) {
            return (std::abs(a) < std::abs(b));
        } // end of lambda expression
    );
}
```

在上面的实例中std::sort函数第三个参数应该是传递一个排序规则的函数，但是这个实例中直接将排序函数的实现写在应该传递函数的位置，省去了定义排序函数的过程，**对于这种不需要复用，且短小的函数，直接传递函数体可以增加代码的可读性。**

### 25.3表达式语法定义

![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/f86a9e30f7474aff10be27e4b51c6f64.png#pic_center)

1.捕获列表。在C ++规范中也称为Lambda导入器， 捕获列表总是出现在Lambda函数的开始处。实际上，==[]是Lambda引出符==。编译器根据该引出符判断接下来的代码是否是Lambda函数，捕获列表能够捕捉上下文中的变量以供Lambda函数使用。

2.*参数列表*。与普通函数的参数列表一致。如果不需要参数传递，则可以连同括号“()”一起省略。

3.可变规则。mutable修饰符， 默认情况下Lambda函数总是一个const函数，mutable可以取消其常量性。在使用该修饰符时，参数列表不可省略（即使参数为空）。

4.*异常说明*。用于Lamdba表达式内部函数抛出异常。

5.返回类型。 追踪返回类型形式声明函数的返回类型。**我们可以在不需要返回值的时候也可以连同符号”->”一起省略**。此外，在返回类型明确的情况下，也可以省略该部分，让编译器对返回类型进行推导。

6.lambda函数体。内容与普通函数一样，不过除了可以使用参数之外，还可以使用所有捕获的变量。

### 25.4 Lambda表达式参数详解

#### 25.4.1捕获列表

Lambda表达式与普通函数最大的区别是，除了可以使用参数以外，Lambda函数还可以**通过捕获列表访问一些上下文中的数据**。具体地，捕捉列表描述了上下文中哪些数据可以被Lambda使用，以及使用方式（以值传递的方式或引用传递的方式）。语法上，在“[]”包括起来的是捕获列表，捕获列表由多个捕获项组成，并以逗号分隔。捕获列表有以下几种形式：

1.[]表示不捕获任何变量

```c++
auto function = ([]{
		std::cout << "Hello World!" << std::endl;
	}
);
function();
```

2.[var]表示值传递方式捕获变量var

```c++
int num = 100;
auto function = ([num]{
		std::cout << num << std::endl;
	}
);

function();
```

3.[=]表示值传递方式捕获所有父作用域的变量（包括this）

```c++
int index = 1;
int num = 100;
auto function = ([=]{
			std::cout << "index: "<< index << ", " 
                << "num: "<< num << std::endl;
	}
);

function();
```

4.[&var]表示引用传递捕捉变量var

```c++
int num = 100;
auto function = ([&num]{
		num = 1000;
		std::cout << "num: " << num << std::endl;
	}
);

function();
```

5.[&]表示引用传递方式捕捉所有父作用域的变量（包括this）

```c++
int index = 1;
int num = 100;
auto function = ([&]{
		num = 1000;
		index = 2;
		std::cout << "index: "<< index << ", " 
            << "num: "<< num << std::endl;
	}
);

function();
```

6.[this]表示值传递方式捕捉当前的this指针

```c++
#include <iostream>
using namespace std;
 
class Lambda
{
public:
    void sayHello() {
        std::cout << "Hello" << std::endl;
    };

    void lambda() {
        auto function = [this]{ 
            this->sayHello(); 
        };

        function();
    }
};
 
int main()
{
    Lambda demo;
    demo.lambda();
}
```

7.[=, &] 拷贝与引用

比如 [=,&a,&b]表示以引用传递的方式捕捉变量a和b，以值传递方式捕捉其它所有变量。

```c++
int index = 1;
int num = 100;
auto function = ([=, &index, &num]{
		num = 1000;
		index = 2;
		std::cout << "index: "<< index << ", " 
            << "num: "<< num << std::endl;
	}
);

function();
```

不过值得注意的是，捕捉列表不允许变量重复传递。下面一些例子就是典型的重复，会导致编译时期的错误。例如：

- [=,a]这里已经以值传递方式捕捉了所有变量，但是重复捕捉a了，会报错的;

- [&,&this]这里&已经以引用传递方式捕捉了所有变量，再捕捉this也是一种重复。

  

如果lambda主体`total`通过引用访问外部变量，并`factor`通过值访问外部变量，则以下捕获子句是等效的：

```c++
[&total, factor]
[factor, &total]
[&, factor]
[factor, &]
[=, &total]
[&total, =]
```

#### 25.4.2 Lambda参数列表

除了捕获列表之外，lambda还可以接受输入参数。参数列表是可选的，并且在大多数方面类似于函数的参数列表。

```c++
auto funcion = [] (int first, int second){
    return first + second;
};
	
function(100, 200);
```

#### 25.4.3可变规格mutable

mutable修饰符， 默认情况下Lambda函数总是一个const函数，mutable可以取消其常量性。在使用该修饰符时，参数列表不可省略（即使参数为空）。

```c++
#include <iostream>
using namespace std;

int main()
{
   int m = 0;
   int n = 0;
   [&, n] (int a) mutable { m = ++n + a; }(4);
   cout << m << endl << n << endl;
}
```

#### 25.4.4异常说明

你可以使用 throw() 异常规范来**指示 lambda 表达式不会引发任何异常**。与普通函数一样，如果 lambda 表达式声明 C4297 异常规范且 lambda 体引发异常，Visual C++ 编译器将生成警告 throw() 。

```c++
int main() // C4297 expected 
{ 
 	[]() throw() { throw 5; }(); 
}
```

#### 25.4.5返回类型

Lambda表达式的**返回类型会自动推导**。除非你指定了返回类型，否则不必使用关键字。返回型类似于通常的方法或函数的返回型部分。但是，返回类型必须在参数列表之后，并且必须在返回类型->之前包含类型关键字。**如果lambda主体仅包含一个return语句或该表达式未返回值，则可以省略Lambda表达式的return-type部分。**如果lambda主体包含一个return语句，则编译器将从return表达式的类型中推断出return类型。否则，编译器将返回类型推导为void。

```c++
auto x1 = [](int i){ return i; };
```

#### 25.4.6 Lambda函数体

```c++
- 捕获变量
- 形参变量
- 局部声明的变量
- 类数据成员，当在类内声明**`this`**并被捕获时
- 具有静态存储持续时间的任何变量，例如全局变量

#include <iostream>
using namespace std;

int main()
{
   int m = 0;
   int n = 0;
   [&, n] (int a) mutable { m = ++n + a; }(4);
   cout << m << endl << n << endl;
}
```

### 25.5适用场景

####25.5.1 Lamdba表达式应用于STL算法库

```c++
// for_each应用实例
int a[4] = {11, 2, 33, 4};
sort(a, a+4, [=](int x, int y) -> bool { return x%10 < y%10; } );
for_each(a, a+4, [=](int x) { cout << x << " ";} );
```

```c++
// find_if应用实例
int x = 5;
int y = 10;
deque<int> coll = { 1, 3, 19, 5, 13, 7, 11, 2, 17 };
auto pos = find_if(coll.cbegin(), coll.cend(), [=](int i) {                 
    return i > x && i < y;
});
```

```c++
// remove_if应用实例
std::vector<int> vec_data = {1, 2, 3, 4, 5, 6, 7, 8, 9};
int x = 5;
vec_data.erase(std::remove_if(vec.date.begin(), vec_data.end(), [](int i) { 
    return n < x;}),
      vec_data.end());

std::for_each(vec.date.begin(), vec_data.end(), [](int i) { 
    std::cout << i << std::endl;});
```



## 26.迭代器   (iterator)

### 26.1定义

迭代器是一种检查容器内元素并遍历元素的数据类型，通常**用于对C++中各种容器内元素的访问**，但不同的容器有不同的迭代器，初学者可以将迭代器理解为**指针**。

### 26.2C++STL常用容器详解

[string容器详解](https://blog.csdn.net/qq_52324409/article/details/121404001)
 [vector容器详解](https://blog.csdn.net/qq_52324409/article/details/121000029)
 [deque容器详解](https://blog.csdn.net/qq_52324409/article/details/121027112)
 [list容器详解](https://blog.csdn.net/qq_52324409/article/details/121046388)
 [set容器详解](https://blog.csdn.net/qq_52324409/article/details/121280952)
 [stack容器详解](https://blog.csdn.net/qq_52324409/article/details/121042345)
 [queue容器详解](https://blog.csdn.net/qq_52324409/article/details/121043685)

### 26.3 bigin()和end()

**begin()就是指向容器第一个元素的迭代器** ,**end()是指向容器==最后一个元素的下一个位置==的迭代器**

```c++
void text()
{
	vector<int> vtr;
	//初始化容器
	for (int i = 0; i < 10; ++i)
	{
		vtr.push_back(i);
	}
	//利用迭代器遍历容器
	cout << "方式1：";
	for (vector<int>::iterator it = vtr.begin(); it != vtr.end(); ++it)
	{
		cout << *it << " ";
	}
	cout << "\n方式1：";
	for (vector<int>::iterator it = begin(vtr); it != end(vtr); ++it)
	{
		cout << *it << " ";
	}
	cout << endl;
}
```

###26.4迭代器类型

按照迭代器的功能强弱，可以把迭代器分为以下几种类型：

1.输入迭代器 （input iterator）

功能：

- 通用的四种功能
- **只能利用迭代器进行输出功能**
- **它只能用于单遍扫描算法**

2. 输出迭代器

功能：

- 通用的四种功能
- **只能利用迭代器进行输入功能**
- **只能用于单遍扫描算法**

3.前向迭代器 （forward iterator）

- 通用的四种功能
- **能利用迭代器进行输入和输出功能**
- **能用于多遍扫描算法**

4.双向迭代器 （bidirectional iterator）

​      通用的四种功能

- 能利用迭代器进行输入和输出功能
- 能用于多遍扫描算法
- **前置和后置递减运算（- -）,这意味这它能够双向访问**

5.随机访问迭代器（ random-access iterator）

通用的四种功能
能利用迭代器进行输入和输出功能
前置和后置递减运算（- -）（意味着它是双向移动的）
比较两个迭代器相对位置的关系运算符（<、<=、>、>=）
支持和一个整数值的加减运算（+、+=、-、-=）
两个迭代器上的减法运算符（-），得到两个迭代器的距离
支持下标运算符（iter[n]），访问距离起始迭代器n个距离的迭代器指向的元素
能用于多遍扫描算法。 在支持双向移动的基础上，支持前后位置的比较、随机存取、直接移动n个距离

总结：

![img](https://img-blog.csdnimg.cn/6de0f723d9324c37a235cd06846bf48f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA77yG5LiN6YCd,size_20,color_FFFFFF,t_70,g_se,x_16)

常用容器的迭代器：

- **vector ——随机访问迭代器**
- **deque——随机访问迭代器**
- **list —— 双向迭代器**
- **set / multiset——双向迭代器**
- **map / multimap——双向迭代器**
- **stack——不支持迭代器**
- **queue——不支持迭代器**

### 26.5通用功能

上面有5种类型的迭代器，我们先来了解一下他们的一些**通用功能**

- **比较两个迭代器是否相等（==、!=）。**
- **前置和后置递增运算（++）。**
- **读取元素的解引用运算符（\*）。只能读元素，也就是解引用只能出现在赋值运算符的右边。**
- **箭头运算符（->），解引用迭代器，并提取对象的成员。**

### 26.6迭代器实例

####26.6.1双向迭代器实例

```c++
void text()
{
	list<int> lst;
	for (int i = 0; i < 10; ++i)
	{
		lst.push_back(i);
	}
	list<int>::iterator it;//创建list的迭代器
	cout << "遍历lst并打印: ";
	for (it = lst.begin(); it != lst.end(); ++it)//用 != 比较两个迭代器
	{
		cout << *it << " ";
	}
	//此时it=lst.end(),这个位置是最后一个元素的下一个位置，没有存储数据
	--it;//等价于it--，回到上一个位置
	//it -= 1; //报错,虽然都是-1，但这种方式是随机迭代器才有的功能
	cout << "\nlst的最后一个元素为：" << *it << endl;
}
```

####26.6.2随机访问迭代器实例

```c++
void text()
{
    vector<int> v;
    for (int i = 0; i < 10; ++i)
    {
        v.push_back(i);
    }
    vector<int>::iterator it;
    for (it = v.begin(); it != v.end(); ++it) //用 != 比较两个迭代器
    {
        cout << *it << " ";
    }
    cout << endl;
    for (it = v.begin(); it < v.end(); ++it) //用 < 比较两个迭代器
    {
        cout << *it << " ";
    }
    cout << endl;
    it = v.begin();//让迭代器重新指向首个元素的位置
    while (it < v.end())//间隔一个输出
    { 
        cout << *it << " ";
        it += 2; // 用 += 移动迭代器
    }
    cout << endl;

    it = v.begin();
    cout << it[5] << endl; //用[]访问
}
```

###26.7vector迭代器失效的情况

对于vector容器来说，其迭代器有失效的可能。
vector容器有==动态扩容==的功能，每当容器容量不足时，vector就会进行动态扩容，动态扩容不是在原来的空间后面追加空间，而是在寻找一段新的更大的空间，把原来的元素复制过去。
但是这样一来，容器存储元素的位置就改变了，原来的迭代器还是指向原来的位置，因此每次进行动态扩容后原来的迭代器就会失效。

### 26.8迭代器的辅助函数

STL 中有用于操作迭代器的三个函数模板，它们是：

```c++
advance(it, n)；使迭代器 it 向前或向后移动 n 个元素。
distance(it1, it2)；计算两个迭代器之间的距离，即迭代器 it1 经过多少次 + + 操作后和迭代器 it2相等。如果调用时 it1 已经指向 it2 的后面，则这个函数会陷入死循环。
iter_swap(it1, it2)；用于交换两个迭代器 it1、it2 指向的值。
```

要使用上述模板，需要包含头文件

```c++
include<algorithm>
```

### 26.9辅助函数实例

```c++
void text1()
{
    int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
    list<int> lst(a, a + 10);
    list<int>::iterator it1 = lst.begin();

    advance(it1, 2);  //it1向后移动两个元素，指向3
    cout << "当前it1指向的元素:" << *it1 << endl;  //输出3
    advance(it1, -1);  //it1向前移动一个元素，指向2
    cout << "当前it1指向的元素:" << *it1 << endl;  //输出2

    list<int>::iterator it2 = lst.end();
    it2--;  //it2指向最后一个元素的位置，即10的位置
    cout << "it1和it2的距离" << distance(it1, it2) << endl;  //输出8

    cout << "交换前打印：";
    for (it1 = begin(lst); it1 != end(lst); ++it1)
    {
        cout << *it1 << " ";
    }
    it1 = begin(lst);
    iter_swap(it1, it2); //交换 1 和 10
    cout << "\n交换后打印：";
    for (it1 = begin(lst); it1 != end(lst); ++it1)
    {
        cout << *it1 << " ";
    }
    cout << endl;
}
```

## 27.模板类 template

1.**模板**是c++支持**参数化多态**的工具，使用模板可以使用户为类或者函数声明一种一般模式，==使得类中的某些数据成员或者成员函数的参数、返回值取得任意类型==。因此可以说，**模板是一种对类型进行参数化的工具**。

2.**template<class T\>** 和 **template<typename T\>** 都可以用来定义**函数模板**和**类模板**，在使用上，他们俩没有本质的区别。

3.函数模板针对仅参数类型不同的函数；类模板针对仅数据成员和成员函数类型不同的类。

### 27.1函数模板

```c++
template< class 形参名，class 形参名，......> 返回类型 函数名(参数列表)   { 函数体 }

举个例子：template <class T> void swap(T& a,T& b){}
```

当调用这样的模板函数时，类型T就会被调用时的类型所代替。如果swap(a, b)，a,b都是int类型，那么模板函数swap中的形参T就会被int所代替，模板函数就会变成swap(int &a,int &b)。而当swap(a,b)，a,b都是double类型，那么模板函数swap中的形参T就会被double所代替，模板函数就会变成swap(double &a,double &b)，这样如果我们的程序中交换变量的值就不再受限于类型了。

### 27.2类模板

```c++
template< class 形参名，class 形参名，......> class 类名 {...};

举个例子：
template <class T> class A { 
    public:
    T a;
    T b;  
    T hy(T c, T &d);
};
```

在类A中声明了两个类型为T的成员变量a和b，还声明了一个返回类型为T带两个参数类型为T的函数hy。



## 28.动态内存

### 28.1堆栈

栈：函数内所有内部声明的变量都是用栈储存的；堆：程序中未使用的内存，在程序运行时可用于动态内存分配。

很多时候我们无法提前知道某个变量的储存空间大小，我们需要来给他分配一个动态储存空间。此时我们就可以用**new**运算符来分给它一个运行内存空间（此空间来自堆），可返回一个当前储存空间的首地址，如果我们不再需要这个动态空间，可用运算符**delete**来将他删掉。

### 28.2 语法

```c++
new data-type;
```

在这里，data-type 可以是包括数组在内的任意数据类型，也可以是包括类

或者结构在内的任何用户自定义的数据类型。

举个例子：

我们定义一个指向  double  类型的指针 ， 然后请求内存，该内存在执行时分配，为了避免空间不够，最好加一个失败返回。

```c++
double* pvalue  = NULL;
if( !(pvalue  = new double ))
{
   cout << "Error: out of memory." <<endl;
   exit(1);
}
delete pvalue; //删除对象
```





## 29.重载运算符

[参考链接](https://blog.csdn.net/arv002/article/details/110004687?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166320575816782248584490%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=166320575816782248584490&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-4-110004687-null-null.142^v47^control,201^v3^control_1&utm_term=C%2B%2B%20%E6%96%B9%E6%B3%95operator%3E%3E&spm=1018.2226.3001.4187)



##28.  size_t 详解

size_t是标准C库中定义的，它是一个基本的与机器相关的**无符号整数的C/C + +类型**， 它是**sizeof操作符返回的结果类型**，该类型的大小可选择。其大小足以保证存储内存中对象的大小（简单理解为 unsigned int就可以了，64位系统中为 long unsigned int）。通常用sizeof(XX)操作，这个操作所得到的结果就是size_t类型。



##29. 回调函数

[参考链接](https://www.runoob.com/w3cnote/c-callback-function.html)



## 30.正则表达式

正则表达式是由字母和符号组成的特殊文本，它从左至右匹配需要的满足某种格式的句子。依赖于元字符。

[参考链接](https://blog.csdn.net/qq_42688149/article/details/117674267?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166467911516782412550355%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166467911516782412550355&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-117674267-null-null.142^v51^control,201^v3^control_2&utm_term=%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%98%AF%E4%BB%80%E4%B9%88&spm=1018.2226.3001.4187)

[正则表达式合集](https://blog.csdn.net/l1028386804/article/details/116778918?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166467208816800184191647%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166467208816800184191647&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-116778918-null-null.142^v51^control,201^v3^control_2&utm_term=%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F&spm=1018.2226.3001.4187)



#31.intn_t

C99标准中inttypes.h的内容
00001
00017
00018 #ifndef __INTTYPES_H_
00019 #define __INTTYPES_H_
00020
00021
00023
00024 typedef signed char int8_t;
00025 typedef unsigned char uint8_t;
00026
00027 typedef int int16_t;
00028 typedef unsigned int uint16_t;
00029
00030 typedef long int32_t;
00031 typedef unsigned long uint32_t;
00032
00033 typedef long long int64_t;
00034 typedef unsigned long long uint64_t;
00035
00036 typedef int16_t intptr_t;
00037 typedef uint16_t uintptr_t;
00038
00039 #endif













































































































































































































































































































































































































