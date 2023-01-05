#  robomaster学习  ——  stl库数据结构算法

——WMJ  陈卓

前言：由于我们比赛的代码基本由C++组成，所以学会运用stl库中的各种函数，算法等内容非常重要。

## 1.迭代器   (iterator)

### 1.1定义

迭代器是一种检查容器内元素并遍历元素的数据类型，通常**用于对C++中各种容器内元素的访问**，但不同的容器有不同的迭代器，初学者可以将迭代器理解为**指针**。

### 1.2 bigin()和end()

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

###1.3迭代器类型

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

### 1.4通用功能

上面有5种类型的迭代器，我们先来了解一下他们的一些**通用功能**

- **比较两个迭代器是否相等（==、!=）。**
- **前置和后置递增运算（++）。**
- **读取元素的解引用运算符（\*）。只能读元素，也就是解引用只能出现在赋值运算符的右边。**
- **箭头运算符（->），解引用迭代器，并提取对象的成员。**

### 1.5迭代器实例

####1.5.1双向迭代器实例

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
	//此时it=lst.end(),这个位置最后一个元素的下一个位置，没有存储数据
	--it;//等价于it--，回到上一个位置
	//it -= 1; //报错,虽然都是-1，但这种方式是随机迭代器才有的功能
	cout << "\nlst的最后一个元素为：" << *it << endl;img
}
```

#### 1.5.2  随机问迭器

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

###1.6  vector迭代器失效的情况

对于vector容器来说，其迭代器有失效的可能。
vector容器有==动态扩容==的功能，每当容器容量不足时，vector就会进行动态扩容，动态扩容不是在原来的空间后面追加空间，而是在寻找一段新的更大的空间，把原来的元素复制过去。
但是这样一来，容器存储元素的位置就改变了，原来的迭代器还是指向原来的位置，因此每次进行动态扩容后原来的迭代器就会失效。

### 1.7  迭代器的辅助函数

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

### 1.8辅助函数实例

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



##2.容器操作

### 2.1概述

C++ STL (标准模板库) 是通用类模板和算法的集合，它提供给程序员一些标准的数据结构的实现，称为容器。STL容器是由一些运用最广的一些数据结构实现出来的。常用的数据结构有array（数组）、vector（向量）、list（列表）、tree（树）、stack（栈）、queue（队列）、hash table（散列表）、set（集合）、map（映射表）等等。

### 2.2C++STL常用容器详解

#### 2.2.1 string 容器用法

**string**是C++中的字符串类型，相当于C语言中的  char*  ，其本质是一个内部封装了char* 的类， 内部封装了很多成员函数用来管理这个字符串，并且不需要担心赋值越界和取值越界等问题。

#####2.2.1.1String的构造函数

```c++
//String的构造函数
string();   //默认构造，创建一个空字符串
string(const char* s);  //使用字符串初始化
string(const string& str);   //拷贝构造，使用一个string 对象初始化另一个string对象
string(int n,char c);  //使用 n 个字符c初始化

//构造函数的使用
string str;
string str("abcde") 或者 string str(s);
string str1(str2);
string str(n,c);

//测试案例
void text01()
{
	string str1;             //1
	string str21("aaaaa");   //2
	string str22="bbbbb";
	string str3(str21);      //3
	string str4(5, 'c');     //4
	cout << "str21=" << str21 <<"\nstr22=" << str22 
		 << "\nstr3=" << str3 << "\nstr4=" << str4 << endl;
}
//输出结果
str21 = aaaaa
str22 = bbbbb
str3 = aaaaa
str4 = ccccc
```

#####2.2.1.2string 赋值操作

```c++
//string 赋值操作
string& operator = (const char*s);    //char* 类型字符串赋给当前字符串
string& operator=(const string& str); //string类型字符串str赋给当前字符串
string& operator=(char c);            //单个字符赋给当前字符串
string& assign(const char* s);        //char*类型字符串赋给当前字符串
string& assign(const char* s,int n);  //把字符串s前n个字符赋给当前字符串
string& assign(const string& str);    //string类型字符串str赋给当前字符串
string& assign(int n,char c);         //把n个字符c赋给当前字符串

//测试案例
void text02()
{
	string str[7];
	str[0] = "aaaaa";              //1
	str[1] = str[0];               //2
	str[2] = 'a';                  //3
	str[3].assign("bbbbb");        //4
	str[4].assign("ccccc", 3);     //5
	str[5].assign(str[4]);         //6
	str[6].assign(5, 'd');         //7
	cout << "\nstr[0]=" << str[0] << "\nstr[1]=" << str[1] << "\nstr[2]=" << str[2] << "\nstr[3]=" << str[3]
		 << "\nstr[4]=" << str[4] << "\nstr[5]=" << str[5] << "\nstr[6]=" << str[6] << endl;
	cout << "str[0][0]=" << str[0][0] << endl;
}
//输出结果
str[0]=aaaaa
str[1]=aaaaa
str[2]=a
str[3]=bbbbb
str[4]=ccc
str[5]=ccc
str[6]=ddddd
str[0][0]=a
```

##### 2.2.1.3string 字符串的拼接

```c++
//string 字符串的拼接
string& operator+=(const char* s);   //重载+=
string& operator+=(const char c);
string& operator+=(const string& str);
string& append(const char* s);       //把字符串s连接到当前字符串尾
string& append(const char* s,int n); //把字符串s前n个字符连接到当前字符串尾
string& append(const string& str);
string& append(const string& str,int pos,int n); //把字符串str从pos开始的n个字符连接到当前字符串尾

//测试案例
void text03()
{
	string str1;
	string str2 = " C++";
	string str3 = " ,yesssssss";
	str1 += 'I';                               //2,
	str1 += " love";                           //1
	str1 += str2;                              //3
	str1.append(" and");                       //4
	str1.append(" algorithm aaa",10);          //5
	str1.append(str3);                         //6
	str1.append(str3, 0, 5);                   //7
	cout << str1;
}
//测试结果
I love C++ and algorithm , yesssssss ,yes
```

##### 2.2.1.4 string查找与替换

```c++
//查找函数`find`和`rfind`，功能是查找指定字符串是否存在，若存在返回出现的下标位置，若不存在返回-1
//替换函数`replace`，在指定位置替换字符串中某些字符

int find(const string& str,int pos = 0);  //从pos开始查找字符串str第一次出现的位置
int find(const char* s,int pos=0);       // 从pos开始查找字符串s第一次出现的位置
int find(const string& str,int pos = 0,int n);//从pos开始查找字符串str的前n个字符第一次出现的位置
int find(const char c,int pos=0);   //从pos开始查找字符c第一次出现的位置
int rfind(const string& str,int pos = npos);  //从pos开始查找字符串str最后一次出现的位置
int rfind(const char* s, int pos = npos); //从pos开始查找字符串s最后一次出现的位置
int rfind(const string& str,int pos,int n);//从pos开始查找字符串str的前n个字符最后一次出现的位置
int rfind(const char c,int pos=0); //从pos开始查找字符c最后一次出现的位置
string& replace(int pos,int n,const string& str);//用字符串str替换从pos开始的n个字符
string& replace(int pos,int n,const char* s);//用字符串s替换从pos开始的n个字符

//测试案例
void text04()
{
	string str1 = "abcdEabcdef";
	int pos = str1.find("cd");
	if (pos != -1)
	{
		cout << "pos=" << pos << endl;
	}
	else cout << "未找到" << endl;

	pos = str1.rfind("cd");
	if (pos != -1)
	{
		cout << "pos=" << pos << endl;
	}
	else cout << "未找到" << endl;

	str1.replace(2, 3, "1234");//用"1234"替换从第二个位置开始的3个字符
	cout << str1 << endl;
}
//测试结果
pos=2
pos=7
ab1234abcdef
```

##### 2.2.1.5 string的比较

```c++
/*比较方式：字符串之间的比较是逐个字符按照ASCII码大小进行比较的，调用str1.compare(str2)成员函数。当两个字符串相等时（即字符串完全相同）函数返回 0 ；
当str1大于str2时函数返回 1；
当str1小于str2时函数返回 -1；*/

int compare(const string& str);  //与字符串str比较
int compare(const char* s);      //与字符串s比较

//特别地，判断两个string类字符串的大小还可以使用重载的运算符 ==、！=、>、<、>=、<= 进行比较。

//测试案例
void text05()
{
	string str1 = "abcdefg";
	string str2 = "bcdef";
	if (str1.compare(str2) == 0)//或者使用 str1==str2
		cout << "字符串相等\n";
	else if (str1.compare(str2) > 0)//或者使用 str1 > str2
		cout << "str1较大\n";
	else
		cout << "str1较小\n";

	if (str1.compare("abcdefg") == 0)
		cout << "字符串相等\n";
	else if (str1.compare("abcdefg") > 0)
		cout << "str1较大\n";
	else
		cout << "str1较小\n";
}
//结果输出
str1较小
字符串相等
```

##### 2.2.1.6string 的存取

```c++
//字符串的存取即 string 中单个字符的访问与修改
char& operator[](int n)  //通过[]方式访问与修改字符
char& at(int n)          //通过at方式获取字符
//测试案例
void text06()
{
	string str = "abcde";
	
	for (int i = 0; i < str.size(); ++i) //size() 是获取字符串大小的函数
	{
		str[i] = 'A'+i;           //通过[]方式修改
		cout << str[i] << " ";    //通过[]方式访问
	}
	cout << endl;
	for (int i = 0; i < str.size(); ++i) 
	{
		str.at(i) = 'a' + i;          //通过at方式修改
		cout << str.at(i) << " ";     //通过at方式访问
	}
}
//结果输出
A B C D E
a b c d e
```

##### 2.2.1.7string 的插入和删除

```c++
string& insert(int pos,const char* s);    // 从pos位置开始插入字符串s，pos均从0开始计数
string& insert(int pos,const string& str);// 从pos位置开始插入字符串str
string& insert(int pos,int n,char c);     //从pos位置开始插入n个字符c
string& erase(int pos,int n=npos);        // 从pos位置开始删除n个字符
//使用：
str.insert(pos,s);
str.insert(pos,str);
str.insert(pos,n,c);
str.erase(pos,n);

//测试案例
void text07()
{
	string str = "hello";
	str.insert(2, "123");
	cout << "插入后：" << str << endl;

	str.erase(2,3);
	cout << "删除后：" << str << endl;
}
//输出结果
插入后： he123llo
删除后： hello
```

##### 2.2.1.8string子串

```C++
//从字符串中获取想要的子串的函数

string substr(int pos,int n=npos);
//返回由pos开始的n个字符组成的子串，默认的npos是从pos到字符串结尾的所有字符的个数

//测试案例
void text08()
{
	//获取QQ邮箱前面的QQ号
	string str = "123456789@qq.com";
	int pos = str.find("@");
	string QQnumb = str.substr(0, pos);
	cout << "邮箱为：" << str << endl;
	cout << "QQ号为：" << QQnumb << endl;
}
//输出结果
邮箱为：123456789@qq.com
QQ号为：123465789
```



#### 2.2.2 vector 容器用法

vector容器的功能和数组非常相似，使用时可以把它看成一个数组。但是数组是静态的，长度不可改变，而vector可以动态扩展（不是在原来的空间上持续加上空间，而是将原数据拷贝到新空间，释放原空间），增加长度。

![在这里插入图片描述](https://img-blog.csdnimg.cn/ca7312c4e8b345ce83a0cc95d66e9d18.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA77yG5LiN6YCd,size_20,color_FFFFFF,t_70,g_se,x_16)

```c++
//打印函数
void printVector(vector<int>& v)
{	//利用迭代器打印 v
	for (vector<int>::iterator it = v.begin(); it != v.end(); ++it)
	{
		cout << *it << " ";
	}
	cout << endl;
}
```



##### 2.2.2.1 vector构造函数

```c++
vector<T> v ; //使用模板类，默认构造函数
vector(v.begin(),v.end()); //将[v.begin(),v.end())区间中的元素拷贝给本身
vextor(n,elem); //将n个elem拷贝给本身
vector(const vector &v) ; //拷贝构造函数

//测试案例
void text01()
{
	vector<int> v1;                       //调用1
	for (int i = 0; i < 5; ++i)
	{
		v1.push_back(i);//向v1末尾添加数据
	}
	vector<int> v2(v1.begin(), v1.end());//调用2
	vector<int> v3(5, 5);                //调用3
	vector<int> v4(v3);                  //调用4
	cout << "打印v2: ";
	printVector(v2);
	cout << "打印v3: ";
	printVector(v3);
	cout << "打印v4: ";
	printVector(v4);
}

//输出结果
打印V2：0 1 2 3 4 
打印V3：5 5 5 5 5 
打印V4：5 5 5 5 5 
```

##### 2.2.2.2  vector的赋值操作

```c++
vector& operator=(const vector &v); //重载赋值运算符
assign(v.begin(),v.end()); //将[v.begin(),v.end())区间中的元素赋值给本身
assign(n,elem); //将n个elem赋值给本身

//测试案例
void text02()
{
	vector<int> v1,v2; 
	for (int i = 0; i < 5; ++i)
	{
		v1.push_back(i);
	}
	v2 = v1;                        //调用1，赋值运算符重载
	vector<int> v3,v4;
	v3.assign(v1.begin(), v1.end());//调用2，区间赋值
	v4.assign(5, 9);                //调用3
	cout << "打印v2: ";
	printVector(v2);
	cout << "打印v3: ";
	printVector(v3);
	cout << "打印v4: ";
	printVector(v4);
}
//输出结果
打印 V2：0 1 2 3 4
打印 V3：0 1 2 3 4 
打印 V4：9 9 9 9 9
```

##### 2.2.2.3  vector的容量与大小

```c++   
empty();         //判断容器是否为空，为空返回1，否则返回0
capacity();      //返回容器的容量
size();          //返回容器的大小,即容器中元素的个数
resize(int num); //重新指定容器的长度为num，若容器变长，则以默认值0填充新位置,如果容器变短，则末尾超过容器长度的元素被删除
resize(int num,int elem); //重新指定容器的长度为num，若容器变长，则以elem填充新位置,如果容器变短，则末尾超过容器长度的元素被删除

//测试案例
void text03()
{
	vector<int> v1;
	if (v1.empty())//调用1，如果容器为空，则给其赋值
	{
		for (int i = 0; i < 5; ++i)
		{
			v1.push_back(i);
		}
	}
	cout << "打印v1: ";
	printVector(v1);
	cout << "v1的容量为：" << v1.capacity() << endl;//调用2
	cout << "v1的大小为：" << v1.size() << endl;//调用3

	//重新指定容器大小使其变长
	v1.resize(10);       //调用4，增加的长度默认值为0
	cout << "调用4,增加长度后,打印v1: ";
	printVector(v1);
	v1.resize(15, 9);    //调用5，增加的长度赋值为9
	cout << "调用5,增加长度后,打印v1: ";
	printVector(v1);
	//重新指定容器大小使其变短
	v1.resize(10);       //调用4，删除了上一步中最后赋值为9的5个长度
	cout << "调用4,减小长度后,打印v1: ";
	printVector(v1);
	v1.resize(5, 9);     //调用5，删除了上一步中默认值为0的5个长度
	cout << "调用5,减小长度后,打印v1: ";
	printVector(v1);
}
//结果输出
打印 V1：0 1 2 3 4
V1的容量为： 6
V1的大小为： 5
调用4，增加长度后，打印V1：0 1 2 3 4 0 0 0 0 0 
调用5，增加长度后，打印V1：0 1 2 3 4 0 0 0 0 0 9 9 9 9 9 
调用4，减小长度后，打印V1：0 1 2 3 4 0 0 0 0 0
调用5，减小长度后，打印V1：0 1 2 3 4
```

##### 2.2.2.4 vector 的插入和删除

```c++
push_back(ele);         //尾部插入元素ele
pop_back();             //删除最后一个元素
insert(const_iterator pos,ele);           //在迭代器指向的位置pos处插入一个元素ele
insert(const_iterator pos,int count,ele); //在迭代器指向的位置pos处插入count个元素ele
erase(const_iterator pos);                    //删除迭代器指向的元素
erase(const_iterator begin,const_iterator end); //删除迭代器从begin到end之间的元素
clear(); //删除容器中所有元素
//测试案例
void text04()
{
	vector<int> v1;
	for (int i = 0; i < 5; ++i)
	{
		v1.push_back(6);//调用1，尾部插入元素6
	}
	cout << "打印v1: ";
	printVector(v1);
	v1.pop_back();//调用2，删除最后一个元素
	cout << "调用2，删除最后一个元素后，打印v1: ";
	printVector(v1);
	v1.insert(v1.begin(),20);//调用3，在首位插入20
	cout << "调用3，在首位插入20，打印v1: ";
	printVector(v1);
	v1.insert(v1.end(), 3, 20);//调用4，在尾部插入3个20
	cout << "调用4，在尾部插入3个20，打印v1: ";
	printVector(v1);
	v1.erase(v1.begin()); //调用5，在首位删除一个元素
	cout << "调用5，在首位删除一个元素，打印v1: ";
	printVector(v1);
	v1.erase(v1.begin(),v1.end()); //调用6，删除首位到末尾所有元素，也就是删除全部元素
	cout << "调用6，删除首位到末尾所有元素，打印v1: ";
	printVector(v1);
	v1.clear();//调用7，清空所有元素
}
//输出结果
打印V1： 6 6 6 6 6 
调用2， 删除最后一个元素后，打印V1： 6 6 6 6
调用3， 在首位插入20，打印V1：20 6 6 6 6 20 20 20
调用4， 在尾部插入3个20，打印V1：20 6 6 6 6 20 20 20 
调用5， 在首位删除一个元素，打印V1：6 6 6 6 20 20 20 
调用6， 删除首位到末尾的所有元素，打印V1：
```

##### 2.2.2.5  vector 数据存取

```c++
at(int idx); //返回索引idx所指的数据
operator[]; //返回[]内索引所指的数据
front(); //返回容器中第一个元素
back(); //返回容器中最后一个元素

//测试案例
void text05()
{
	vector<int> v;
	for (int i = 0; i < 5; ++i)
	{
		v.push_back(i);
	}
	//利用at访问v
	cout << "调用1，打印v: ";
	for (int i = 0; i < v.size(); ++i)
	{
		cout << v.at(i) << " ";//调用1
	}
	cout << endl;
	//利用[]访问v
	cout << "调用2，打印v: ";
	for (int i = 0; i < v.size(); ++i)
	{
		cout << v[i] << " ";//调用2
	}
	cout << endl;
	cout << "容器中第一个元素是：" << v.front() << endl;//调用3
	cout << "容器中最后一个元素是：" << v.back() << endl;//调用4
}
//结果输出
调用1，打印： 0 1 2 3 4
调用2，打印： 0 1 2 3 4
容器中的第一个元素是： 0
容器中的最后一个元素是： 4     
```

##### 2.2.2.6 vector 互换容器

```c++
swap(v); //容器v和当前容器互换

//测试案例
void text06_1()
{
	vector<int> v1,v2;
	for (int i = 0; i < 10; ++i)
	{
		v1.push_back(i);
		v2.push_back(9 - i);
	}
	cout << "交换前：" << endl;
	printVector(v1);
	printVector(v2);
	cout << "交换后：" << endl;
	v1.swap(v2);   //调用互换函数
	printVector(v1);
	printVector(v2);
}
//结果输出
交换前：
0 1 2 3 4 5 6 7 8 9 
9 8 7 6 5 4 3 2 1 0 
交换后：
9 8 7 6 5 4 3 2 1 0
0 1 2 3 4 5 6 7 8 9 
```

##### 2.2.2.7 vector 预留空间

功能：减少vector在动态扩容时的扩展次数
每次使用push_back(v)时，如果容器v的大小要超过v的容量时，系统就会对v进行一次动态扩容，至于扩大多少空间，由系统决定。

```c++
reserve(int len);//容器预留len个元素长度，也就是把容量扩为len，预留的位置并不初始化，同时也不可访问

//测试案例
void text07()
{
	vector<int> v1,v2;
	v2.reserve(10000);//给v2设置预留空间,v1不设置

	//对比v1,v2，系统需要动态扩容多少次
	int num1 = 0;       //扩容次数
	int num2 = 0;       
	int capacity1 = 0;  //容量大小
	int capacity2 = 0;
	for (int i = 0; i < 10000; ++i)
	{
		v1.push_back(i);
		if (capacity1 != v1.capacity())//计算扩容次数,当容量不相等时，证明系统扩容了
		{
			capacity1 = v1.capacity();
			++num1;
		}

		v2.push_back(i);
		if (capacity2 != v2.capacity())
		{
			capacity2 = v2.capacity();
			++num2;
		}
	}
	cout << "系统对v1扩容" << num1 << "次" << endl;
	cout << "系统对v2扩容" << num2 << "次" << endl;
}
//输出结果
系统对V1扩容24次
系统对V23扩容1次
```



####2.2.3 deque 容器用法

deque 是一个双端数组 ， 可以对头端和尾端进行插入和删除操作。

![在这里插入图片描述](https://img-blog.csdnimg.cn/031f80b28cbf4eabb17c3d30ecb82930.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA77yG5LiN6YCd,size_20,color_FFFFFF,t_70,g_se,x_16)

deque和vector的区别：deque对于头部的插入和删除效率比vector高，vector访问元素时速度比deque快，这和两者的内部实现有关

deque内部工作原理：
deque内部有个中控器，维护每段缓冲区中的内容，缓冲区中存放数据。中控器维护的是每个缓冲区的地址，使得使用deque时像一片连续的内存空间![在这里插入图片描述](https://img-blog.csdnimg.cn/7de7a48750bf4be78c3dffe00356f691.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA77yG5LiN6YCd,size_20,color_FFFFFF,t_70,g_se,x_16)

```c++
//打印函数
void printDeque(const deque<int>& d)//const限制了只能读取数据
{
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}
```



#####2.2.3.1  deque的构造函数

```c++
deque<T> d;             //使用模板类，默认构造函数
deque(begin(),end());   //利用迭代器将所有元素拷贝给本身
deque(n,elem);          //将n个elem拷贝给本身
deque(const deque& d);  //拷贝构造函数

//测试案例
void text01()
{
	deque<int> d1;                           //调用1
	for (int i = 0; i < 5; i++)
	{
		d1.push_back(i);
	}
	deque<int> d2(d1.begin(), d1.end());     //调用2
	cout << "打印d2: ";
	printDeque(d2);
	deque<int> d3(5, 6);                     //调用3
	cout << "打印d3: ";
	printDeque(d3);
	deque<int> d4(d3);                       //调用4
	cout << "打印d4: ";
	printDeque(d4);
}
//输出结果
打印d2: 0 1 2 3 4
打印d3: 6 6 6 6 6 
打印d4: 6 6 6 6 6
```

##### 2.2.3.2 deque的赋值操作

```c++
deque& operator=(const deque &d); //重载赋值运算符
assign(begin(),end());            //将全部数据拷贝给本身
assign(n,elem);                    //将n个elem赋值给本身
//测试案例
void text02()
{
	deque<int> d1, d2, d3, d4;
	for (int i = 0; i < 5; i++)
	{
		d1.push_back(i);
	}
	d2=d1;                               //调用1
	cout << "打印d2: ";
	printDeque(d2);
	d3.assign(d1.begin(), d1.end());     //调用2
	cout << "打印d3: ";
	printDeque(d3);
	d4.assign(5, 9);                     //调用3
	cout << "打印d4: ";
	printDeque(d4);
}
//结果输出
打印d2:0 1 2 3 4 
打印d3:0 1 2 3 4
打印d4:9 9 9 9 9 
```

##### 2.2.3.3  deque的大小操作

```c++
empty();      //判断容器是否为空，是返回1，否返回0
size();       //返回容器中元素个数
resize(num);  //重新指定容器长度为num,若容器变长，以默认值0填充新位置
//若容器变短，末尾超出容器长度的部分被删除
4.resize(num,elem); //重新指定容器长度为num,若容器变长，以elem填充新位置
//若容器变短，末尾超出容器长度的部分被删除

//注意：和vector不同的是，deque是没有容量概念的，也就是没有capacity()这个接口
    
//测试案例
void text03()
{
	deque<int> d1;
	for (int i = 0; i < 5; i++)
	{
		d1.push_back(i);
	}
	if (!d1.empty())//调用1，若容器非空
	{
		cout << "容器的大小为：" << d1.size() << endl;//调用2
		cout << "打印d1: ";
		printDeque(d1);
	}
	//重新指定容器长度
	d1.resize(10,5); //调用4        
	cout << "打印d1: ";
	printDeque(d1);
	d1.resize(8);  //调用3
	cout << "打印d1: ";
	printDeque(d1);
}
//结果输出：
容器的大小为: 5
打印d1: 0 1 2 3 4
打印d1: 0 1 2 3 4 5 5 5 5 5 
打印d1: 0 1 2 3 4 5 5 5
```

##### 2.2.3.4 deque 的插入与删除

```c++
push_back(elem);       //在容器尾部插入一个数据
push_front(elem);      //在容器头部插入一个数据
pop_back();            //删除容器最后一个数据
pop_front();           //删除容器第一个数据
insert(pos,elem);      //在迭代器pos位置插入一个elem数据，返回新数据位置
insert(pos,n,elem);    //在迭代器pos位置插入n个elem数据，无返回值
insert(pos,begin,end); //在迭代器pos位置插入[begin,end)区间的数据，无返回值
erase(pos);            //删除迭代器pos位置的数据，返回下一个数据位置
erase(begin,end);      //删除[begin,end)区间的数据，返回下一个数据的位置
clear();               //清空所有数据

//测试案例
void text04()
{
	deque<int> d1;
	for (int i = 0; i < 5; i++)
	{
		d1.push_back(i + 5);//调用1
		d1.push_front(5 - i);//调用2
	}
	cout << "打印d1: ";
	printDeque(d1);
	d1.pop_back();//调用3
	d1.pop_front();//调用4
	cout << "打印d1: ";
	printDeque(d1);
	d1.insert(d1.begin(), 5, 3);//调用6
	cout << "打印d1: ";
	printDeque(d1);
	d1.erase(d1.begin()); //调用8
	cout << "打印d1: ";
	printDeque(d1);
	d1.clear();        //调用10
	cout << "打印d1: ";
	printDeque(d1);
}
//结果输出
打印d1: 1 2 3 4 5 5 6 7 8 9
打印d1: 2 3 4 5 5 6 7 8
打印d1: 3 3 3 3 3 2 3 4 5 5 6 7 8
打印d1: 3 3 3 3 2 3 4 5 5 6 7 8
打印d1: 
```

##### 2.2.2.5 deque 的数据存取

```c++
at(int idx); //返回索引idx所指的数据
operator[]; //重载[]
front();   //返回容器中第一个数据元素
back();    //返回容器中最后一个数据元素

//测试案例
void text05()
{
	deque<int> d;
	for (int i = 0; i < 5; i++)
	{
		d.push_back(i);
	}
	for (int i = 0; i < d.size(); i++)
	{
		cout << d.at(i);    //调用1
		cout << d[i] << " ";//调用2
	}
	cout << endl;
	cout << "容器d的第一个元素是：" << d.front() << endl;//调用3
	cout << "容器d的最后一个元素是：" << d.back() << endl;//调用4
}
//结果输出
00 11 22 33 44 
容器d的第一个元素是:0
容器d的最后一个元素是：4
```



#### 2.2.4 list容器用法

list一种将数据进行链式存储的数据结构，被称为链表.(是一种物理存储单元上非连续的存储结构，数据元素的逻辑顺序是通过链表中的指针链接实现的)。

优点：可以在任意位置快速插入和删除元素，采用动态存储分配，不会造成内存浪费和溢出。

缺点：占用空间比数组大，访问元素效率低，需要遍历链表才能访问元素。

STL中的list容器是一个双向循环链表

```c++
//打印函数
void printList(const list<int>& lst)
{
	cout << "打印list: ";
	for (list<int>::const_iterator it = lst.begin(); it != lst.end(); it++)
	{
		cout << *it << " ";
	}
	cout << endl;
}
```



##### 2.2.4.1 lsit构造函数

```c++
 list<T> lst;            //默认构造，list采用模板类实现
 list(begin,end);        //构造函数，利用迭代器实现区间赋值
 list(n,elem);           //构造函数，将n个elem赋值给本身
 list(const list &lst);  //拷贝构造
//测试案例
void text1()
{
	list<int> lst1;                            //默认构造
	for (int i = 1; i < 10; ++i)
	{
		lst1.push_back(i);
	}
	printList(lst1);

	list<int> lst2(lst1.begin(), lst1.end());  //区间赋值的构造函数
	printList(lst2);

	list<int> lst3(9, 1);                      //将9个1赋值给本身
	printList(lst3);

	list<int> lst4(lst1);                      //拷贝构造
	printList(lst3);
}
//结果输出
打印list:1 2 3 4 5 6 7 8 9 
打印list:1 2 3 4 5 6 7 8 9
打印list:1 1 1 1 1 1 1 1 1 
打印list:1 1 1 1 1 1 1 1 1
```

##### 2.2.4.2 list 的赋值和交换

```c++
 list& operator=(const list& lst);  //重载赋值运算符
 assign(begin,end);                 //利用迭代器实现区间赋值
 assign(n,elem);                    //将n个elem赋值给本身
 swap(lst);                         //将lst于本身互换
//测试案例
void text2()
{
	list<int> lst1, lst2, lst3, lst4;
	for (int i = 1; i < 10; ++i)
	{
		lst1.push_back(i);
	}
	printList(lst1);

	lst2 = lst1;                           //重载的 = 赋值
	printList(lst2);

	lst3.assign(lst2.begin(), lst2.end()); //区间赋值
	printList(lst3);

	lst4.assign(9, 1);                     //将9个1赋值给本身
	printList(lst4);

	//list交换
	lst1.swap(lst4);
	cout << "交换后：" << endl;
	printList(lst1);
	printList(lst4);
}
//结果输出
打印list:1 2 3 4 5 6 7 8 9
打印list:1 2 3 4 5 6 7 8 9
打印list:1 2 3 4 5 6 7 8 9
打印list:1 1 1 1 1 1 1 1 1
交换后:  
打印list:1 1 1 1 1 1 1 1 1
打印list:1 2 3 4 5 6 7 8 9
```

#####2.2.4.3  list的大小操作

```c++
 size();            //返回容器中元素个数
 empty();           //判断容器是否为空
 resize(num);       //重新指定容器长度,若变长，变长的部分赋值为0,
                      //若变短，超出容器长度的元素被删除
 resize(num,elem);  //重新指定容器长度,若变长，变长的部分赋值为elem
                      //若变短，超出容器长度的元素被删除
//测试案例
void text3()
{
	list<int> lst1;
	for (int i = 1; i < 10; ++i)
	{
		lst1.push_back(i);
	}
	printList(lst1);

	if (!lst1.empty())  //容器若不为空
	{
		cout << "list容器的大小为：" << lst1.size() << endl;
	}
	
	cout << "重新指定长度，使容器变长: " << endl;
	lst1.resize(15);     //等价于lst1.resize(15,0);
	printList(lst1);

	cout << "重新指定长度，使容器变短: " << endl;
	lst1.resize(5);     
	printList(lst1);
}
//结果输出
打印list: 1 2 3 4 5 6 7 8 9
list容器的大小为：9
重新指定长度，使容器变长：
打印list:1 2 3 4 5 6 7 8 9 0 0 0 0 0 0
重新指定长度，使容器变短：
打印list:1 2 3 4 5 
```

##### 2.2.4.4 list 的插入和删除

```c++
 1.push_back(elem);      //在容器尾部插入一个元素
 2.push_front(elem);     //在容器头部插入一个元素
 3.pop_back();           //在容器尾部删除一个元素
 4.pop_front();          //在容器头部删除一个元素
 5.insert(pos,elem);     //在迭代器pos处插入一个元素elem,返回新数据的位置
 6.insert(pos,n,elem);   //在迭代器pos处插入n个元素elem,无返回值
 7.insert(pos,begin,end);//在迭代器pos处插入[begin,end)区间的数据,无返回值
 8.clear();              //清除容器中所有数据
 9.erase(begin,end);     //删除[begin,end)区间的数据,返回下一个数据的位置
 10.erase(pos);          //删除迭代器pos处的数据,返回下一个数据的位置
 11.remove(elem);        //删除容器中所有等于elem的元素

//测试案例
void text4()
{
	list<int> lst1;
	for (int i = 1; i < 6; ++i)
	{
		lst1.push_back(i+5);     //在容器尾部插入一个元素
		lst1.push_front(6 - i);  //在容器头部插入一个元素
	}
	printList(lst1);

	lst1.pop_back();        //在容器尾部删除一个元素
	lst1.pop_front();       //在容器尾部删除一个元素
	printList(lst1);

	list<int> lst2;
	//在lst2头部插入lst1的全部数据
	lst2.insert(lst2.begin(),lst1.begin(), lst1.end());
	lst2.insert(lst2.end(), 5, 10);  //在lst2尾部插入5个10
	printList(lst2);

	lst2.remove(10);   //删除lst2中等于10的元素
	printList(lst2);
	
	//删除lst2中所有元素
	lst2.erase(lst2.begin(), lst2.end());   //等价于lst2.clear();
	printList(lst2);
}
//结果输出
打印list:1 2 3 4 5 6 7 8 9 10
打印list:2 3 4 5 6 7 8 9
打印list:2 3 4 5 6 7 8 9 10 10 10 10 10
打印list:2 3 4 5 6 7 8 9 
打印list:
```

##### 2.2.4.5 list的数据存取

```c++
 1.front();   //返回容器第一个元素
 2.back();    //返回容器最后一个元素
//测试案例
void text5()
{
	list<int> lst1;
	for (int i = 1; i < 10; ++i)
	{
		lst1.push_back(i);
	}
	printList(lst1);
	cout << "容器中第一个元素是：" << lst1.front() << endl;
	cout << "容器中最后一个元素是：" << lst1.back() << endl;
}
//结果输出
打印list: 1 2 3 4 5 6 7 8 9
容器中的第一个元素是:1
容器中的最后一个元素是:9
```

##### 2.2.4.6 list 的反转与排序

```c++
1.reverse(); //反转链表
2.sort();    //排序链表。注意这是一个成员函数，测试中会解释
//测试案例
void text6()
{
	list<int> lst1;
	lst1.push_back(5);
	lst1.push_back(1);
	lst1.push_back(7);
	lst1.push_back(3);
	lst1.push_back(6);
	printList(lst1);
	//反转
	cout << "反转后：" << endl;
	lst1.reverse();
	printList(lst1);
	//排序
	//sort(lst1.begin(),lst1.end())//错误
	//所有不支持随机访问迭代器的容器，不能使用标准算法
	//但不支持随机访问迭代器的容器，内部会提供一些算法来弥补
	lst1.sort();  //list容器内部的排序算法
	printList(lst1);
}
//结果输出
打印list: 5 1 7 3 6
反转后：
打印list: 6 3 7 1 5
打印list: 1 3 5 6 7
```



####2.2.5 stack容器用法

stack是一种先进先出的数据结构，被称为栈，它只有一端可以出入。
栈中进入数据称为——入栈(push)
栈中弹出数据称为——出栈(pop)
![在这里插入图片描述](https://img-blog.csdnimg.cn/f59b9079b52b4fe384b97f5489f32b39.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA77yG5LiN6YCd,size_20,color_FFFFFF,t_70,g_se,x_16)

```c++
//构造函数
stack<T> stk;            //stack采用模板类实现，默认构造，T为数据类型，如int等
stack(const stack &stk); //拷贝构造

//赋值操作
stack& operator=(const stack& stk);//重载赋值运算符

//数据存取
push(elem); //入栈，向栈顶添加元素
pop(); //出栈，删除栈顶元素
top(); //返回栈顶元素

//大小操作
empty(); //判断栈是否为空
size(); //返回栈的大小

//测试案例
void text()
{
	stack<int> stk1;
	for (int i = 0; i < 5; ++i)
	{
		stk1.push(i+10);
	}
	stack<int> stk2(stk1);
	cout << "stk1的大小为：" << stk1.size() << endl;
	cout << "stk2的大小为：" << stk2.size() << endl;
	//打印栈顶元素并出栈
	while (!stk1.empty())//栈非空时
	{
		cout << "stk1栈顶元素为：" << stk1.top() << endl;
		stk1.pop();
	}
	cout << "stk1的大小为：" << stk1.size() << endl;
	stk2 = stk1;
	cout << "stk2的大小为：" << stk2.size() << endl;
}
//结果输出
stk1的大小为：5
stk2的大小为：5
stk1的栈顶元素为：14
stk1的栈顶元素为：13
stk1的栈顶元素为：12
stk1的栈顶元素为：11
stk1的栈顶元素为：10
stk1的大小为：0
stk2的大小为：0
```



####2.2.6 queue容器用法

queue是一种先进先出的数据结构，被称为队列，它的一端为入口(队尾入队)，另一端为出口(队首出队) 。队列中进数据称为——入队(push)，队列中出数据称为——出队(pop)。

![在这里插入图片描述](https://img-blog.csdnimg.cn/80bd8f5db54c4df5bc25edd3523e312c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA77yG5LiN6YCd,size_20,color_FFFFFF,t_70,g_se,x_16)

```c++
构造函数
queue<T> que;                  //queue采用模板类实现，默认构造函数
queue(const queue& que);       //拷贝构造

赋值操作
queue& operator=(const queue& que);//重载赋值运算符

数据存取：
push(elem);        //入队——往队尾添加元素
pop();             //出队——从队头删除元素
back();            //返回队尾元素
front();           //返回队头元素

大小操作：
empty();           //判断队列是否为空
size();            //返回队列大小
//测试案例
class Person
{
public:
	string m_name;
	int m_age;
	Person(string name, int age)
	{
		m_name = name;
		m_age = age;
	}
};
int main()
{
	Person p1("张三", 35);
	Person p2("李四", 28);
	Person p3("王五", 30);
	Person p4("赵六", 40);
	queue<Person> que;
	que.push(p1);
	que.push(p2);
	que.push(p3);
	que.push(p4);
	cout << "队列的大小为：" << que.size() << endl;
	while (!que.empty())
	{
		cout << "队头元素为：" << que.front().m_name 
			 << que.front().m_age << "岁" << endl;
		cout << "队尾元素为：" << que.back().m_name 
			 << que.back().m_age << "岁" << endl;
		que.pop();
	}
	cout << "队列的大小为：" << que.size() << endl;
}
//结果输出
队列的大小为：4
队头元素为:张三35岁
队尾元素为:赵六40岁
队头元素为:李四28岁
队尾元素为:赵六40岁
队头元素为:王五30岁
队尾元素为：赵六40岁
队头元素为：赵六40岁
队尾元素为：赵六40岁
队列的大小为：0
```

## 3.STL常用算法

C++STL中的内置算法主要在头文件\<algorithm>、\<functional>、\<numeric>中\<algorithm>是所有STL头文件中最大的一个，也是包含算法最多，最常用的一个头文件，其中包含比较、交换、查找、遍历、复制、修改等算法。

###3.1最大最小值算法

```c++
max(a,b)    //返回a,b中较大的数
min(a,b)    //返回a,b中较小的数

使用这两个函数时，不能使用与其同名的变量(max、min)，比如 max = max(a, b)、min = min(min, b)等。
这两个算法只能比较两个数的大小，如果想比较两个以上数的大小可以嵌套使用，比如求三个数中的最大值m = max(a, max(b, c))
    
//实例
#include<iostream>
#include<algorithm>
using namespace std;
int main()
{
	int arr[10] = { 5,8,2,4,6,9,7,0,1,3 };
	int m1=arr[0];
	int m2 = arr[0];
	for (int i = 0; i < 10; ++i) {
		m1 = max(m1, arr[i]);
		m2 = min(m2, arr[i]);
	}
	cout << "最大值为: " << m1 
		 << "\n最小值为: " << m2 << endl;
}
//结果输出
最大值：9
最小值：0
```

### 3.2常用遍历算法

####3.2.1  for_each

功能： 遍历容器，进行某种**自定义操作**。

```c++
for_each (iterator beg,iterator end,_func);  // 遍历容器

第一个参数beg可以传入容器的起始迭代器，也可以传入数组内第一个元素的地址
第二个参数end可以传入容器的结束迭代器，或者传入数组最后一个元素的下一个位置的地址
最后一个参数_func可以传入一个函数或者函数对象
//对于一个数组而言，数组内第一个元素的地址相当于起始迭代器，数组最后一个元素的下一个位置的地址相当于结束迭代器。
```

#### 3.2.2 transform

按照某种**自定义规则**搬运容器(或数组)内元素到另一个容器(或数组)中。

```c++
transform(iterator beg1,iterator end1,iterator beg2,_func);

beg1传入原容器的起始迭代器，或数组的首地址
end1传入原容器的结束迭代器，或者数组最后一个元素的下一个位置的地址
beg2传入目标容器的起始迭代器，或者目标数组的首地址
_func传入搬运规则函数或函数对象
```


```c++
//测试实例
void printArr(int val)//自定义操作：打印
{
	cout << val << " ";
}
int targetArr(int val)//搬运规则函数，在原数据基础上加100
{
	return val + 100;
}
int main()
{
	int arr[10] = { 0,1,2,3,4,5,6,7,8,9 };//原数组
	int target[15];//目标数组,目标数组的长度必须不小于原数组
	transform(arr, arr + 10, target, targetArr);
	cout << "原数组打印：";
	for_each(arr, arr + 10, printArr);
	cout << "\n目标数组打印：";
	for_each(target, target + 10, printArr);
}
//结果输出
原数组打印：0 1 2 3 4 5 6 7 8 9
目标数组打印：100 101 102 103 104 105 106 107 108 109
```

### 3.3 常用查找方法

```c++
find   //查找元素
find_if //按条件查找元素
adjacent_find //查找相邻重复元素
binary_find  //二分查找法查找元素
count   //统计元素个数
count_if  //按条件统计元素个数
```

#### 3.3.1 find

按值查找元素

```c++
find(iterator beg,iterator end,value);

beg起始迭代器
end结束迭代器
value是要查找的元素，如果其是自定义数据类型，如类的对象、结构体类型的元素,查找时需要重载==运算符
找到返回指定位置的迭代器，找不到返回结束迭代器
    
//测试实例1
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
int main()
{
	vector<int> v;
	for (int i = 0; i < 10; ++i) {
		v.push_back(i);
	}
	auto it = find(v.begin(), v.end(), 5);//查找元素5
	if (it != v.end()) {
		cout << "找到元素" << *it << endl;
	}
	else {
		cout << "未找到元素";
	}
}
//输出结果
找到元素5
    
//测试实例2
class Person
{
public:
	string m_name;
	int m_age;
	Person(string name,int age):m_name(name),m_age(age){}
	//重载==运算符,否则find不知道按什么规则比较
	bool operator==(const Person& p)
	{
		if (m_name == p.m_name && m_age == p.m_age) {
			return true;
		}
		return false;
	}
};
int main()
{
	vector<Person> v;
	v.push_back(Person("张三", 20));
	v.push_back(Person("李四", 25));
	v.push_back(Person("王五", 30));
	v.push_back(Person("赵六", 40));

	Person p("王五",30);

	auto it = find(v.begin(), v.end(), p);//查找元素5
	if (it != v.end()) {
		cout << "找到此人:" << it->m_name << it->m_age << endl;
	}
	else {
		cout << "未找到此人";
	}
}
//输出结果
找到此人：王五30
```

#### 3.3.2  find_if

按指定规则查找元素。

```c++
find_if(iterator beg,iterator end,_Pred);

//  beg起始迭代器
//  end结束迭代器
//  _Pred称为谓词(返回值是bool类型的函数或仿函数称为谓词),在这里用做查找规则。 谓词这个概念很重要，此后会经常见到
//   找到返回指定位置的迭代器，找不到返回结束迭代器

//测试实例
bool greaterFive(int val)//查找规则
{
	return val > 5;
}
int main()
{
	vector<int> v;
	for (int i = 0; i < 10; ++i) {
		v.push_back(i);
	}
	auto it = find_if(v.begin(), v.end(), greaterFive);
	if (it != v.end()) {
		cout << "找到大于5的元素：" << *it << endl;
	}
	else {
		cout << "未找到" << endl;
	}
}
//结果输出
找到大于5的元素：6
```

#### 3.3.3 adjacent_find

查找相邻且重复的元素。

```c++
adjacent_find(iterator beg,iterator end);

//beg起始迭代器
//end结束迭代器
//找到返回相邻元素第一个元素位置的迭代器，没找到返回容器结束迭代器

//测试实例
int main()
{
	vector<int> v;
	v.push_back(2);
	v.push_back(5);
	v.push_back(2);
	v.push_back(3);
	v.push_back(3);
	auto it = adjacent_find(v.begin(), v.end());
	if (it != v.end()) {
		cout << "相邻重复的元素为：" << *it << endl;
	}
	else {
		cout << "不存在相邻重复的元素" << endl;
	}
}
//输出结果
相邻重复的元素为：3
```

#### 3.3.4 binary_finid

查找指定元素value，**找到返回true，否则返回false**。查找速度快，但是只能查找有序序列。

```c++
bool binary_search(iterator beg,iterator end,value);

//beg起始迭代器
//end结束迭代器
//只能在有序序列中使用，如果用于无序序列，函数返回的结果将不准确

//测试实例
int main()
{
	vector<int> v;
	for (int i = 0; i < 10; ++i) {
		v.push_back(i);
	}
	bool flag = binary_search(v.begin(), v.end(), 5);
	if (flag == 1) {
		cout << "找到指定元素" << endl;
	}
	else {
		cout << "未找到" << endl;
	}
}
//结果输出
找到指定元素
```

#### 3.3.5 count

统计相同元素出现次数。

```c++
count(iterator beg,iterator end,value);

//统计元素value的个数
//beg起始迭代器
//end结束迭代器

//测试案例
class Person
{
public:
	string m_name;
	int m_age;
	Person(string name,int age):m_name(name),m_age(age) {}
	bool operator==(const Person& p)//重载==，作为统计元素规则
	{
		if (m_age == p.m_age) {
			return true;//统计年龄相同的元素
		}
		return false;
	}
};
int main()
{
	vector<Person> v;
	v.push_back(Person("张三", 30));
	v.push_back(Person("李四", 25));
	v.push_back(Person("王五", 35));
	v.push_back(Person("赵六", 25));
	v.push_back(Person("熊大", 25));
	v.push_back(Person("熊二", 20));

	Person p("光头强", 25);
	int num = count(v.begin(), v.end(), p);
	cout << "年龄为25的生物的个数：" << num << endl;
}
//结果输出
年龄为25的生物的个数：3
```

#### 3.3.6 count_if

按照指定条件统计元素个数

```c++
count_if(iteraotr beg,iterator end,_Pred);

//beg起始迭代器
//end结束迭代器
//_Pred 谓词，在这里用作统计的条件

//测试实例
bool greater5(int val)
{
	return val > 5;
}
int main()
{
	vector<int> v;
	for (int i = 0; i < 10; ++i) {
		v.push_back(i);
	}
	int num = count_if(v.begin(), v.end(), greater5);
	cout << "大于5的元素的个数为：" << num << endl;
}
//输出结果
大于5的元素的个数为：4
```

### 3.4常用排序算法

```c++
sort            //对容器内元素进行排序
random_shuffle  //对指定范围内元素随机调整次序
merge           //容器元素合并，并存储到另一容器中
reverse         //反转指定范围的元素
```

#### 3.4.1 sort

对容器或数组内元素进行排序

是C++STL中最常用的算法,最快,其**内部主要由快速排序+插入排序+堆排序实现的**。

```c++
sort(iterator beg,iterator end,_Pred);

//默认对容器内元素升序排序(从小到大)
//beg起始迭代器
//end结束迭代器
//_Pred 谓词，可以不填这个参数(默认升序排序)，如果想按照其他规则排序，如降序排序，这个参数就必须填入
//这个算法只有随机存取迭代器可以使用，如普通数组、vector容器、deque容器等等

//测试案例
void myPrint(int val)
{
	cout << val << " ";
}
int targetVector(int val)
{
	return val;
}
bool myComepare(int v1,int v2)
{
	return v1 > v2;
}
int main()
{
	int a[10] = { 1,4,7,2,5,8,3,6,9 };//乱序数组
	vector<int> v(10);
	transform(a, a + 10, v.begin(), targetVector);

	cout << "排序前打印数组a: ";
	for_each(a, a + 10, myPrint);
	cout << "\n排序前打印容器v: ";
	for_each(v.begin(), v.end(), myPrint);

	sort(a, a + 10);//默认升序排序
	cout << "\n升序排序后打印数组a: ";
	for_each(a, a + 10, myPrint);

	sort(v.begin(), v.end(), myComepare);//降序排序
	cout << "\n降序排序后打印容器v: ";
	for_each(v.begin(), v.end(), myPrint);
}
//结果输出
排序前打印数组a: 1 4 7 2 5 8 3 6 9 0
排序前打印容器v: 1 4 7 2 5 8 3 6 9 0
升序排序后打印数组a: 0 1 2 3 4 5 6 7 8 9
降序排序后打印容器v: 9 8 7 6 5 4 3 2 1 0
```

#### 3.4.2 random_shuffle

指定范围内元素随机调整次序.

```c++
random_shuffle(iterator beg,iterator end);

// beg起始迭代器
// end结束迭代器
// 指定范围[beg,end)内元素随机打乱
注意： 在使用random_shuffle之前最好先撒下随机数种子srand((unsigned)time(NULL))，否则每次随机的结果都是一样的。
    
//测试案例
 void myPrint(int val)
{
	cout << val << " ";
}
int main()
{
	int a[10] = { 0,1,2,3,4,5,6,7,8,9 };
	srand((unsigned)time(NULL));//随机数种子
	random_shuffle(a, a + 10);
	cout << "随机打乱后：";
	for_each(a, a + 10, myPrint);
}
//输出结果
随机打乱后：0 5 4 6 7 1 8 3 9 2  //随机的结果
```

#### 3.4.3  merge

将两个**有序**的容器合并到另一个目标容器中，**合并后该目标容器仍然有序**

```c++
merge(iterator beg1,iterator end1,iterator beg2,iterator end2,iterator target_beg);

//beg1第一个容器或数组起始迭代器
//end1第一个容器或数组结束迭代器
//beg2第一个容器或数组起始迭代器
//end2第一个容器或数组结束迭代器
//target_beg目标容器或数组起始迭代器

注意：两个容器必须是有序的并且是同一种顺序，不能一个升序一个降序，目标容器的大小必须提前指定，并且其大小不小于合并的那两个容器的大小之和

//测试案例
void myPrint(int val)
{
	cout << val << " ";
}
int main()
{
	vector<int> v;
	int a[10] = { 0,1,2,3,4,5,6,7,8,9 };
	
	for (int i = 0; i < 10; ++i) {
		v.push_back(i + 5);
	}
	vector<int> target(10 + v.size());
	cout << "打印a: ";
	for_each(a,a+10, myPrint);
	cout << "\n打印打印v2: ";
	for_each(v.begin(), v.end(), myPrint);

	merge(a, a + 10, v.begin(), v.end(), target.begin());
	cout << "\n打印合并后容器target: ";
	for_each(target.begin(), target.end(), myPrint);
}
//结果输出
打印a: 0 1 2 3 4 5 6 7 8 9
打印打印v2: 5 6 7 8 9 10 11 12 13 14
打印合并后容器target: 0 1 2 3 4 5 5 6 6 7 7 8 8 9 9 10 11 12 13 14
```

#### 3.4.4  reverse

将容器内元素进行反转。

```c++
int main()
{
	int arr[10] = { 0,1,2,3,4,5,6,7,8,9 };
	reverse(arr, arr + 10);
	cout << "反转后输出：";
	for (int i = 0; i < 10; ++i) {
		cout << arr[i] << " ";
	}
}
//输出结果
反转后输出：9 8 7 6 5 4 3 2 1 0
```

