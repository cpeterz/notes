# C++多线程（std::thread)



## 1.多线程

C++11提供了语言层面上的多线程，包含在头文件<thread>中。C++11 新标准中引入了5个头文件来支持多线程编程，thread  mutex  atomic   condition_variable  future

### 1.1多线程与多进程

#### 多进程并法

将一个应用程序划分为多个独立的进程（单线程），彼此之间可以相互通信，共同完成任务。但由于要避免一个进程修改另一个进程的数据，操作系统加了很多保护机制，这就导致其使用比较复杂且速度较慢

#### 多线程并法

线程依赖于进程，同一个进程中的多个线程享有相同的地址空间。

### 1.2创建线程

方式多样简单，只需把函数添加到线程中去。

注：thread_fun是我要加入到线程中的函数

例一：std::thread myThread ( thread_fun);                    //函数形式为void thread_fun()
            myThread.join();                                                     //同一个函数可以代码复用，创建多个线程

例二：std::thread myThread(thread_fun(100));            //函数形式为void thread_fun(int)

例三：std::thread(thread_fun,1).detach()                      //直接创建现线程，没有名字

### 1.3join与detach方式

线程启动后，一定要在和线程相关联的thread销毁前，确定以何种方式等待线程执行结束

- detach方式，启动的线程自主在后台运行，当前的代码继续往下执行，不等待新线程结束。
- join方式，等待启动的线程完成，才会继续往下执行。

### 1.4this_thread

this_thread是一个类，它有4个功能函数，具体如下：
函数	                    使用	                                                                                 说明
get_id	   std::this_thread::get_id() 	                                                     获取线程id
yield	     std::this_thread::yield()	                                               放弃线程执行，回到就绪状态
sleep_for	std::this_thread::sleep_for(std::chrono::seconds(1));	        暂停1秒
sleep_until           	如下	                                                                    一分钟后执行吗，如下

------------------------------------------------
using std::chrono::system_clock;
std::time_t tt = system_clock::to_time_t(system_clock::now());

struct std::tm * ptm = std::localtime(&tt);
cout << "Waiting for the next minute to begin...\n";
++ptm->tm_min; //加一分钟
ptm->tm_sec = 0; //秒数设置为0
//暂停执行，到下一整分执行
this_thread::sleep_until(system_clock::from_time_t(mktime(ptm)));





