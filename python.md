## python =速成

## 1.输出

作用：程序输出内容给用户

```python
print('hello Python')
age = 19
print(age)

name = 'Tom'
print('我的名字是%s' % name)
print(f'我的名字是{name}')

```

在Python中，print()， 默认自带`end="\n"`这个换行结束符，所以导致每两个`print`直接会换行展示，用户可以按需求更改结束符。



## 2.输入

在Python中，程序接收用户输入的数据的功能即是输入。

`input("提示信息")`

输入的特点

1.当程序执行到input，等待用户输入，输入完成之后才继续向下执行。
2.在Python中，input接收用户输入后，一般存储到变量，方便使用。
3.在Python中，input会把接收到的任意用户输入的数据都当做字符串处理。

```python
name = input('请输入您的名字：')
print(f'您输入的名字是{name}')
print(type(name))

password = input('请输入您的密码：')
print(f'您输入的密码是{password}')
print(type(password))
```



 ## 3. 转换数据类型常用的函数

- int()   将x转换为一个整数
- float()    将x转换为一个浮点数
- str()   将对象 x 转换为字符串
- list()    将序列 s 转换为一个列表
- tuple()   将序列 s 转换为一个元组
-  eval()  用来计算在字符串中的有效Python表达式,并返回一个对象



## 4.逻辑运算符

运算符	逻辑表达式	描述	实例
and	    x and y	     布尔"与"：如果 x 为 False，x and y 返回 False，否则它返回 y 的值。	    True and False， 返回 False。
or	x or y	布尔"或"：如果 x 是 True，它返回 True，否则它返回 y 的值。	False or True， 返回 True。
not	not x	布尔"非"：如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。	not True 返回 False, not False 返回 True



## 5.if else

```python
age = int(input('请输入您的年龄：'))
if age < 18:
    print(f'您的年龄是{age},童工一枚')
elif (age >= 18) and (age <= 60):
    print(f'您的年龄是{age},合法工龄')
elif age > 60:
    print(f'您的年龄是{age},可以退休')
```



## 6.三目运算符

值1 if 条件 else 值2

```python
a = 1
b = 2

c = a if a > b else b
print(c)
```



## 7.循环

**while**

```python
while 条件1:
    条件1成立执行的代码
    ......
    while 条件2:
        条件2成立执行的代码
        ......
```

**for**

```python
for 临时变量 in 序列:
    重复执行的代码1
    重复执行的代码2
    ......
```

**else**

循环可以和else配合使用，else下方缩进的代码指的是==当循环正常结束之后要执行的代码==。

**while...else**

```python
i = 1
while i <= 5:
    ...
else:
    ...
```

for...else

```python
for 临时变量 in 序列:
    重复执行的代码
    ...
else:
    循环正常结束之后要执行的代码
```

==continue退出循环的方式执行else下方缩进的代码==





##8.切片

切片是指对操作的对象截取其中一部分的操作。字符串、列表、元组都支持切片操作。

序列[开始位置下标:结束位置下标:步长]

    注意
    不包含结束位置下标对应的数据， 正负整数均可；
    步长是选取间隔，正负整数均可，默认步长为1。

体验

```python
name = "abcdefg"

print(name[2:5:1])  # cde
print(name[2:5])  # cde
print(name[:5])  # abcde
print(name[1:])  # bcdefg
print(name[:])  # abcdefg
print(name[::2])  # aceg
print(name[:-1])  # abcdef, 负1表示倒数第一个数据
print(name[-4:-1])  # def
print(name[::-1])  # gfedcba
```


## 9.列表

```html
[数据1, 数据2, 数据3, 数据4......]
```

列表可以一次性存储多个数据，且**可以为不同数据类型**。

**下标**   [n]

**函数**  

1. index() 返回指定数据所在位置的下标 。

   列表序列.index(数据, 开始位置下标, 结束位置下标)

2. count()：统计指定数据在当前列表中出现的次数。

3. len()：访问列表长度，即列表中数据的个数。

4. 判断是否存在

   in：判断指定数据在某个列表序列，如果在返回True，否则返回False

   not in：判断指定数据不在某个列表序列，如果不在返回True，否则返回False

   ```python
       name_list = ['Tom', 'Lily', 'Rose']
       ​
       # 结果：True
       print('Lily' in name_list)
       ​
       # 结果：False
       print('Lilys' in name_list)
   ```

5. append()：列表结尾追加数据。

6. extend()：列表结尾追加数据，如果数据是一个序列，则将这个序列的数据逐一添加到列表。

7.  insert()：指定位置新增数据。  列表序列.insert(位置下标, 数据)

8. del()： 删除  del 目标















































