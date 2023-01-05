## C++:mutex库

#### 1.std::mutex

最基本的mutex类，在多线程环境中，有多个线程竞争同一个公共资源，就很容易引发线程安全的问题。因此就需要引入锁的机制，来保证任意时候只有一个线程在访问公共资源。

```c++
#include<thread>
#include<mutex>
#include<iostream>

void Add(int* px, int n)
{
	for (int i = 0; i < n; i++)
	{
		(*px)++;
	}
}

void Fun()
{
	int x = 0;
	std::cout << "Before Add:x=" << x << std::endl;
	std::thread t1(Add, &x, 10000000);
	std::thread t2(Add, &x, 10000000);
	t1.join();
	t2.join();
	std::cout << "After Add:x=" << x << std::endl;
	std::cout << "----------------------------"<<std :: endl;
}
int main()
{
	for (int i = 0; i < 5; i++){
		std::cout << "--------AddNum =" << i <<"-----------"<< std::endl;
		Fun();
	}
	system("pause");
	return 0;
}

// 执行结果
--------AddNum =0-----------
Before Add:x=0
After Add:x=15703953
----------------------------
--------AddNum =1-----------
Before Add:x=0
After Add:x=12994172
----------------------------
--------AddNum =2-----------
Before Add:x=0
After Add:x=14373455
----------------------------
--------AddNum =3-----------
Before Add:x=0
After Add:x=12553528
----------------------------
--------AddNum =4-----------
Before Add:x=0
After Add:x=13493504
----------------------------

```

####1.1互斥锁

可以发现，在多线程时，当数据量较大时，每次执行结果都不一样，并且结果不同。

则应采用**加锁方式**来解决，以下是两种加锁方式：

方式一：

```c
void Add1(int* px, int n,std::mutex* pmt)
{
	(*pmt).lock();
	for (int i = 0; i < n; i++)
	{
		(*px)++;
	}
	(*pmt).unlock();
}
```

方式二：

```c
void Add2(int* px, int n, std::mutex* pmt)
{
	for (int i = 0; i < n; i++)
	{
		(*pmt).lock();
		(*px)++;
		(*pmt).unlock();
	}
}
```

调用函数改为：

```c
std::mutex mt;
std::thread t1(Add2, &y, 10000000, &mt);
std::thread t2(Add2, &y, 10000000, &mt);
```

第一种加锁方式中，第一个线程进去之后，另一个线程阻塞休眠，在第一个线程完成循环后，再唤醒另外一个线程。

第二种方式是在每次加加循环前后选择一个线程并关闭其他线程。

综合来看，第一种方式所花费的时间远远少于第二种。

#### 1.2自旋锁

互斥锁和自选锁：

==互斥锁==：
lock() ：第一个线程进去后，后面的线程会进入阻塞休眠状态，被放入到阻塞队列中。
unlock()：加锁的线程执行完后，会解锁，然后去阻塞队列中唤醒阻塞线程。
适用于锁的粒度大的场景

==自旋锁==：
lock()：第一个线程进去，后面的线程在循环空转检查。
unlock()：第一个加锁的线程解锁，后面的线程检测到就可以被cpu调度。

互斥锁就像给计算机一个电话号码，线程先去睡觉，等可以执行了再去打电话把线程叫醒；而自选锁就是线程每隔一小段时间就去看看能不能开始执行了。所以互斥锁适合时间长的线程，而自旋锁适合时间跨度小的线程。











































