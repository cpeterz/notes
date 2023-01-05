# ROS

ROS的设计目的是：简化在各种机器人平台上创建复杂而强大的机器人行为的任务即不重复造轮子！

#1. 入门

##1.1  ROS2系统架构

系统操作层： 基于Linux、Windows或者macOS系统建立

DDS实现层： 对不同常见的DDS接口进行再次的封装，让其保持统一性，为DDS抽象层提供统一的API。

抽象DDS层（RMW）： 这一层将DDS实现层进一步的封装，使得DDS更容易使用。

(DDS组成了ROS2的中间层，实现了去中心化的传输功能)

ROS2客户端：ROS的客户端库就是RCL，不同的语言对应着不同的rcl，但基本功能都是相同的。

应用层：应用层就是我们写代码以及ROS2开发的各种常用的机器人相关开发工具所在的层了。后面我们写的所有代码其实都是属于这一层的。

## 1.2 基础知识

### 1.2.1 库

程序编译一般需要经==预处理、编译、汇编和链接==几个步骤。有些==公共代码需要反复使用==，就把这些代码编译成为“库”文件。

==静态（链接）库==：链接步骤中，链接器将从库文件取得所需的代码，复制到生成的可执行文件中。

==动态（链接）库==：程序在开始运行后调用库函数时才被载入，这种库独立于现有的程序，其本身不可执行，但包含着程序需要调用的一些函数。

### 1.2.2 g++

-I  寻找头文件目录

-L  制定库文件目录

-l(小写L)  制定库文件名字

  ### 1.2.3 make

**批处理**工具，我们可以将g++的指令写成脚本放在Makefile中，就可以通过make自动的调用脚本完成操作。

###1.2.4  Cmake

cmake 工具可以直接生成 Makefile

find_package(  )     链接头文件

target_link_libraries(  )      链接库

###1.2.5  节点

四种通信方式：

- 话题-topics
- 服务-services
- 动作-Action
- 参数-parameters

```c++
//启动包下的节点
ros2 run <package_name> <executable_name>
//查看节点列表(常用)：
ros2 node list
//查看节点信息(常用)
ros2 node info <node_name>：
//重映射节点名称
ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle
//运行节点时设置参数
ros2 run example_parameters_rclcpp parameters_basic --ros-args -p rcl_log_level:=10    
```

###1.2.6  工作空间

一个工作空间下可以有多个功能包，一个功能包可以有多个节点存在。

### 1.2.7 功能包

ament_python，适用于python程序

cmake，适用于C++

ament_cmake，适用于C++程序,是cmake的增强版

### 1.2.8  功能包获取

```c++
//安装获取会自动放置到系统目录，不用再次手动source。
sudo apt install ros-<version>-package_name
//手动编译相对麻烦一些，需要下载源码然后进行编译生成相关文件。
手动编译之后，需要手动source工作空间的install目录。    
```

### 1.2.9  与功能包相关的指令

```c++
create       Create a new ROS2 package
executables  Output a list of package specific executables
list         Output a list of available packages
prefix       Output the prefix path of a package
xml          Output the XML of the package manifest or a specific tag
```

```c++
//创建功能包
ros2 pkg create <package-name>  --build-type  {cmake,ament_cmake,ament_python}  --dependencies <依赖名字>
//列出可执行文件,列出所有
ros2 pkg executables
//列出turtlesim功能包的所有可执行文件
ros2 pkg executables turtlesim    
//列出所有的包
ros2 pkg list
//输出某个包所在路径的前缀  
ros2 pkg prefix  <package-name>
//列出包的清单描述文件
每一个功能包都有一个标配的manifest.xml文件，用于记录这个包的名字，构建工具，编译信息，拥有者，干啥用的等信息。
ros2 pkg xml turtlesim  
```

## 1.3. Colcon

功能包构建工具,作用是编译代码。

```c++
colcon build 
```

### 1.3.1 编完之后的目录结构

```c++
.
├── build
├── install
├── log
└── src

4 directories, 0 files
```

1.   `build` 目录存储的是中间文件。对于每个包，将创建一个子文件夹，在其中调用例如CMake。
2. `install` 目录是==每个软件包将安装到的位置==。默认情况下，每个包都将安装到单独的子目录中。

3.  `log` 目录包含有关每个colcon调用的各种日志信息。

### 1.3.2 运行一个自己编的节点

**1.进入我们刚刚创建的工作空间，先source 一下资源**

```c++
source install/setup.bash
```

**2.运行一个订杂志节点**

```C++
ros2 run examples_rclcpp_minimal_subscriber subscriber_member_function
```

**3.打开一个新的终端，先source，再运行一个发行杂志节点**

### 1.3.3 指令

**1.只编译一个包**

```c++
colcon build --packages-select YOUR_PKG_NAME 
```

**2.不编译测试单元**

```c++
colcon build --packages-select YOUR_PKG_NAME  --cmake-args -DBUILD_TESTING=0
```

**3.运行编译的包的测试**

```c++
colcon test
```

**4.允许通过更改src下的部分文件来改变install**

```c++
colcon build --symlink-install
```

## 1.4  编写节点

### 1.4.1 创建 功能包

```c++
//创建example_cpp功能包，使用ament-cmake作为编译类型，并为其添加rclcpp依赖。
cd chapt2_ws/src
ros2 pkg create village_wang --build-type ament_cmake --dependencies rclcpp
```

1.   --pkg create 是==创建包==的意思

2.  --build-type 用来==指定该包的编译类型==，一共有三个可选项`ament_python`、`ament_cmake`、`cmake`

3. --dependencies 指的是这个==功能包的依赖==，这里给了一个ros2的cpp客户端接口`rclcpp`

打开终端，进入`chapt2_ws/src`运行上面的指令，创建完成后的目录结构如下：

![](/home/peter/图片/ros节点创建.png)

### 1.4.2 创建节点

在`village_wang/src`下创建一个`wang2.cpp`文件.

![](/home/peter/图片/ros节点创建2.png)

### 1.4.3 添加到CmakeLists

在wang2.cpp中输入想要实现的代码。加入下列两行

```c++
add_executable(wang2_node src/wang2.cpp)
//生成可运行文件
ament_target_dependencies(wang2_node rclcpp)
//添加依赖
```

以上两行实现了编译，但编译好的文件需要安装到`install/village_wang/lib/village_wang`下

```c++
install(TARGETS
  wang2_node
  DESTINATION lib/${PROJECT_NAME}
)
```

### 1.4.4 编译运行节点

**编译节点**                                                             

进入工作空间

```c++
colcon build
```

**source环境**

```c++
source install/setup.bash
```

**运行节点**

```c++
ros2 run village_wang wang2_node
```

### 1.4.5 测试

当节点运行起来后，可以再尝试使用`ros2 node list `指令来查看现有的节点。这个时候你应该能看到：



#2.进阶

## 2.1 Topic  通信

节点创建一个发布者（Publisher）发布话题（Topic），另一个节点创建订阅者（Subscriber）来订阅。

### 2.1.1 规则

可以一对一，也可以一对n，n对一，n对n。

1. ==话题名字是关键,发布订阅接口类型要相同==，发布的是字符串，接受也要用字符串来接收;
2. 同一个人(节点)可以订阅多个话题，同时也可以发布多个话题，就像一本书的作者也可以是另外一本书的读者;
3. 同一个小说不能有多个作者（版权问题），但跟小说不一样，同一个话题可以有多个发布者。

### 2.1.2 相关工具

**1.  RQT工具之 rqt_graph**

```
rqt_graph
```

ROS2作为一个强大的工具，在运行过程中，我们是可以通过命令来看到节点和节点之间的数据关系的。

**2. 话题相关指令工具**

```c++
//展示所有topic 相关指令
ros2 topic -h
//返回当前所有活动的主题列表
ros2 topic list
//返回当前所有活动的主题列表（加消息类型）
ros2 topic list -t
//打印实时话题内容
ros2 topic echo /chatter
//查看主题信息
ros2 topic info  /chatter
//查看消息类型
ros2 interface show std_msgs/msg/String
//手动发布命令
ros2 topic pub arg
例：ros2 topic pub /chatter std_msgs/msg/String 'data: "123"'
```

### 2.1.3 继承Node类

继承Node类后会获得其能力：

1.创建一个话题订阅者的能力，用于拿到艳娘传奇的数据

```c++
rclcpp::Subscription<std_msgs::msg::String>::SharedPtr sub_;
sub_ = this->create_subscription<std_msgs::msg::String>("sexy_girl", 10, std::bind(&SingleDogNode::topic_callback, this, _1));
```

2.创建一个话题发布者的能力，用于给李四送稿费

```c++
rclcpp::Publisher<std_msgs::msg::UInt32>::SharedPtr pub_;
pub_ = this->create_publisher<std_msgs::msg::UInt32>("sexy_girl_money",10);
```

3.获取日志打印器的能力

```c++
// 打印一句自我介绍
RCLCPP_INFO(this->get_logger(), "大家好，我是单身汉王二.");
```

### 2.1.4 代码实现

创建话题订阅者的一般流程：

1. **导入订阅的话题接口类型**

   ```c++
   #include "std_msgs/msg/string.hpp"
   #include "std_msgs/msg/u_int32.hpp"
   ```

2. **创建订阅回调函数**

   std::bind()

   成员函数作为回调函数，不能像普通函数一样直接用，因为它需要实例化对象。一种解决方式是把该函数设计为静态，但显然会破坏类的结构性。也可以实例化后将指针作为参数传递给它。最后一种方法就是==使用std::bind 和std::function 结合实现回调函数==。

3. **声明并创建订阅者**   this->create_subscription  

   C++中创建一个订阅者，需要传入话题类型、话题名称、所要绑定的回调函数，以及通信Qos

4. **编写订阅回调处理逻辑**

### 2.1.5 流程

两个节点间要实现通信，首先需要一个节点来发布消息，面向对象编程。

1.需要创建发布者（`rclcpp::Publisher<std_msgs::msg::发布类型>::SharedPtr 发布者名字;`）并放在类的私有属性中。 

2.构造函数放在共有属性中，在构造函数中初始化发布者`发布者名字 = this->create_publisher<std_msgs::msg::发布类型>("话题名称", 10);`

3.因为在发布的时候我们需要设定发布时间间隔，所以应该在此处选用回调函数`时间名称 = this->create_wall_timer(std::chrono::milliseconds(时间间隔长度/ms), std::bind(&结构体名称::定时器回调函数名称, this));`

4.在私有属性中就加入回调函数

```c++
//例子：
void timer_callback()
    {
        //创建消息
        std_msgs::msg::String novel;
        novel.data = std::string("第"+std::to_string(i)+"册");
        //日志打印
        RCLCPP_INFO(this->get_logger(), "已发布%s.", novel.data.c_str(),i++);
        //发布消息
        pub_novel->publish(novel);
    }
```

5.此时另一个节点作为就需要接受消息。同理，首先在私有属性中声明订阅者，`rclcpp::Subscription<std_msgs::msg::订阅类型>::SharedPtr 订阅者名称;`

6.然后在公有函数中创建订阅者，`订阅者名称 = this->create_subscription<std_msgs::msg::订阅类型>(”话题名称“, 10, std::bind(&类的名称::话题回调函数, this, _1(传入参数));`同样使用了回调函数，目的是每次接收到消息后能够输出一定反馈方便调试，同样，我们可以再在此回调函数中进行发布，相当于反馈。

7.编写回调函数，同样放到私有属性中

```c++
void topic_callback(const std_msgs::msg::String::SharedPtr msg)
    {
        std_msgs::msg::UInt32 money;
        money.data = 10;
        //发送人民币给李四
        pub_money->publish(money);
        RCLCPP_INFO(this->get_logger(), "朕已阅%s,累积打赏李四：%d 元的稿费", msg->data.c_str(), money.data);
    };
```

8.如果回调中又发布了，则下一个订阅者同理。

9.主函数

```c++
int main(int argc, char **argv)
{
    rclcpp::init(argc, argv);
    auto node = std::make_shared<PoorNode>(传入参数);
    rclcpp::spin(node);
    rclcpp::shutdown();
    return 0;
}
```

10.类需要继承

```c++
class PoorNode : public rclcpp::Node
{
public:
    PoorNode(std::string name) : Node(name)
    {
    }
}
```

11. 输出节点之间的关系图像

```c++
//终端输入：
qt_graph    
```

### 2.1.6 测试订阅

```c++
ros2 topic pub  /sexy_girl_money std_msgs/msg/UInt32 "{data: 10}"
```

## 2.2 接口

### 2.2.1 ros2接口介绍

ros2给了很多接口，这些接口使得不同语言能够实现相互通信，相互理解。

使用`ros2 interface package 接口包名`命令可以查看某一个接口包下所有的接口。

```c++
//比如
打开终端输入：ros2 interface package sensor_msgs
sensor_msgs/msg/JointState  #机器人关节数据
sensor_msgs/msg/Temperature #温度数据
sensor_msgs/msg/JoyFeedbackArray 
sensor_msgs/msg/Joy
sensor_msgs/msg/PointCloud2 #点云
sensor_msgs/msg/MultiEchoLaserScan 
sensor_msgs/msg/NavSatStatus 
sensor_msgs/msg/CameraInfo #相机信息
sensor_msgs/msg/Illuminance 
sensor_msgs/msg/MagneticField
sensor_msgs/srv/SetCameraInfo
sensor_msgs/msg/LaserEcho 
sensor_msgs/msg/RegionOfInterest
sensor_msgs/msg/PointCloud #点云
sensor_msgs/msg/Range #范围
sensor_msgs/msg/RelativeHumidity
sensor_msgs/msg/FluidPressure
sensor_msgs/msg/BatteryState
sensor_msgs/msg/Imu #加速度传感器
sensor_msgs/msg/Image #图像
sensor_msgs/msg/PointField
sensor_msgs/msg/JoyFeedback
sensor_msgs/msg/LaserScan #雷达数据
sensor_msgs/msg/MultiDOFJointState #多自由度关节数据
sensor_msgs/msg/TimeReference 
sensor_msgs/msg/CompressedImage #压缩图像
sensor_msgs/msg/NavSatFix 
sensor_msgs/msg/ChannelFloat32
```

```c++
ros2 interface show sensor_msgs/msg/Image─
此消息包含未压缩的图像
#(0,0)在图像的左上角
std_msgs/Header Header # Header时间戳应该是图像的采集时间
标题frame_id应该是相机的光学帧
#边框原点应为相机的光学中心
# +x应该指向图像的右边
# +y应该在图像中指向下方
# +z应该指向图像的一个平面
#如果这里的frame_id和CameraInfo的frame_id
#与图像冲突相关的消息
#行为未定义
Uint32 height #图像高度，即行数
Uint32 width #图像宽度，即列数
#编码的合法值在文件src/image_encoding .cpp中
如果你想标准化一个新的字符串格式，加入
# ros-users@lists.ros.org，并发送一封电子邮件建议一个新的编码。
像素的编码——通道的含义，顺序，大小
#取自include/sensor_msgs/image_encoding .hpp中的字符串列表
Uint8 is_bigendian #这个数据是双位数的吗?
uint32 step #行长度(以字节为单位)
Uint8 [] data #实际矩阵数据，大小是(step * rows)
```

### 2.2.2 自定义接口

ROS2提供了四种通信方式：

- 话题-Topics
- 服务-Services
- 动作-Action
- 参数-Parameters

其中服务，动作，话题均可以使用自定义接口。

话题接口格式：`xxx.msg`

```
int64 num
```

服务接口格式：`xxx.srv`

```
int64 a
int64 b
---
int64 sum
```

动作接口格式：`xxx.action`

```c++
int32 order
---
int32[] sequence
---
int32[] partial_sequence
```

通过 ROS2 的IDL模块，可以将msg、srv、action文件转换为Python和C++的头文件。

### 2.2.3 ROS2 接口常用 CLI命令

**查看接口列表**

```c++
ros2 interface list
```

**查看所有接口包**

```c++
ros2 interface packages 
```

**查看某一个包下的所有接口**

```c++
ros2 interface package 包名(std_msgs)
```

**查看某一个接口详细的内容**

```c++
ros2 interface show 接口(std_msgs/msg/String)
```

**输出某一个接口所有属性**

```c++
ros2 interface proto 接口（sensor_msgs/msg/Image）
```

### 2.2.4 自定义话题接口

定义话题接口==只需要定义发布者所要发布的类型==即可。

在实际的工程当中，为了==减少功能包互相之间的依赖==，通常会==将接口定义在一个独立的功能包中==，所以小鱼会新建一个叫做`village_interfaces`的功能包，并把所有ROS镇下的接口都定义在这个独立的功能包里。

新建方法如下：

- **1.新建msg文件夹，并在文件夹下新建`xxx.msg`**

  ![](/home/peter/图片/ros2自定义话题.png)

  注意:==msg文件开头首字母一定要大写==，ROS2强制要求。

  注:

- 2.**在xxx.msg下编写消息内容并保存**

  以字符串和图片接口举例子：

  ```c++
  //发布`std_msgs/msg/String`字符串类型的数据和`sensor_msgs/msg/Image`图片类型的数据。
  //标准消息接口std_msgs下的String类型
  std_msgs/String content
  //图像消息，调用sensor_msgs下的Image类型
  sensor_msgs/Image image
  ```

  ![](/home/peter/图片/话题接口结构图.png)

​     第一层为消息定义层，第二层为ROS2已有的std_msgs,sensor_msgs，第三层是ROS2的原始数据类型。

​    ROS2中的原始数据类型包括：

```c++
bool
byte
char
float32,float64
int8,uint8
int16,uint16
int32,uint32
int64,uint64
string
```

**另一种写法**：

采用ROS2的原始类型来组合。 即不使用std_msgs/String 而是直接使用最下面一层的string。

![](/home/peter/图片/话题接口结构图2.png)

文件内容就变成了这样：

```c++
//直接使用ROS2原始的数据类型
string content
//图像消息，调用sensor_msgs下的Image类型
sensor_msgs/Image image
```

- **3.在CMakeLists.txt添加依赖和msg文件目录**

  我们需要在CMakeLists.txt中的 .msg 文件转换成 C++ 的头文件。

  ```c++
  //找sensor_msgs对应的包
  find_package(sensor_msgs REQUIRED)
  //转换包rosidl_default_generators，它根据消息接口文件生成用于特定语言（如C++，Python等）的代码，便于在这些语言里使用消息接口文件
  find_package(rosidl_default_generators REQUIRED)
  //rosidl_generate_interfaces就是声明msg文件所属的工程名字、文件位置以及依赖DEPENDENCIES。
  rosidl_generate_interfaces(${PROJECT_NAME}
    "msg/Novel.msg"
     DEPENDENCIES sensor_msgs
   )
  //重点强调依赖部分DEPENDENCIES，我们消息中用到的依赖这里必须写上，即使不写编译器也不会报错，直到运行的时候才会出错。这个部分要放在ament_package()后面。
  ```

- **4.在package.xml中添加xxx.msg所需的依赖**

  修改village_interfaces目录下的`package.xml`，添加下面三行代码，为工程添加一下所需的依赖。

  ```c++
    <depend>sensor_msgs</depend>
    <build_depend>rosidl_default_generators</build_depend>
    <exec_depend>rosidl_default_runtime</exec_depend>
    <member_of_group>rosidl_interface_packages</member_of_group>
  ```

  添加完后方在这里：

  ![](/home/peter/图片/package.xml修改.png)

- **5.编译功能包即可生成python与c++头文件**

  ```c++
  //回到工作空间
  colcon build --packages-select 接口包名
  ```

  **6.检验**

  ```c++
  source install/setup.bash 
  ros2 interface package village_interfaces  #查看包下所有接口
  ros2 interface show village_interfaces/msg/Novel #查看内容
  ros2 interface proto village_interfaces/msg/Novel #显示属性
  ```

## 2.3  Service 介绍

客户端发送请求给服务端，服务端可以根据客户端的请求做一些处理，然后返回结果给客户端。

话题是没有返回的，适用于单向或大量的数据传递。而==服务是双向的，客户端发送请求，服务端响应请求==。

注意事项：

- 同一个服务（名称相同）有且只能有一个节点来提供
- 同一个服务可以被多个客户端调用

### 2.3.1 ROS2 服务常用命令

**查看服务列表**

```c++
ros2 service list
```

**手动调用服务**

```
ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{a: 5,b: 10}"
```

**查看服务接口类型**

```
ros2 service type /add_two_ints
```

**查找使用某一接口的服务**

```
ros2 service find 接口
```

###2.3.2  服务接口介绍

话题是**发布订阅模型**。主要是单向传输数据，只能由发布者发布，接收者接收（同一话题，发布者接收者都可以有多个）。

服务是**客户端服务端（请求响应）模型**。由客户端发送请求，服务端处理请求，然后返回处理结果（同一服务，客户端可以由多个，服务端只能有一个）。

服务接口格式：`xxx.srv`

```c++
int64 a
int64 b
---   //横线上方为客户端，横线下端是服务端
int64 sum
```

### 2.3.3 服务端实现

1.**添加依赖**

（添加依赖是为了让程序能够在编译和运行时找到对应的接口）

如果包类型是ament_cmake ，则需要进行以下两步操作：

第一步：修改 package.xml ，加入下列代码：

```xml
  <depend>village_interfaces</depend>
```

![](/home/peter/图片/xml依赖添加.png)

第二步：修改 CMakeLists.txt

加入下列代码：

```cmake
#查找库
find_package(village_interfaces REQUIRED)
#添加链接
ament_target_dependencies(wang2_node 
  rclcpp 
  village_interfaces   #这是新加的
)
```

**2.添加服务接口**

只须在程序中引入对应的头文件即可。

```c++
//这里举一个自定义的接口的例子，这个sell_novel.hpp是编译自定义接口后形成的头文件
#include "village_interfaces/srv/sell_novel.hpp"
```

**3.声明回调函数**

传入的指针分别是请求指针和反馈指针

```c++
// 声明一个回调函数，当收到买书请求时调用该函数，用于处理数据
void sell_book_callback(const village_interfaces::srv::SellNovel::Request::SharedPtr request,
        const village_interfaces::srv::SellNovel::Response::SharedPtr response)
{
}
```

**4.多线程死锁**

服务经常会存在死锁状态，即两个线程相互等待的情况，我们需要用多线程来解决这个问题。

回调函数的使用需要用到回调组：

```c++
命名空间// 声明一个服务回调组
rclcpp::CallbackGroup::SharedPtr callback_group_service_;
```

**5.声明服务端**

```c++
rclcpp::Service<接口>::SharedPtr 服务端对象;
```

**6.实例化服务端并编写回调函数处理请求**

服务端和回调组都需要实例化：

```c++
//实例化回调组
callback_group_service_ = this->create_callback_group(rclcpp::CallbackGroupType::MutuallyExclusive);
//实例化服务端
server_ = this->create_service<接口>("服务名称",std::bind(&回调函数,this,命名空间),rmw_qos_profile_services_default,callback_group_service_);
//rmw_qos_profile_services_default 通信质量，这里使用服务默认的通信质量
//callback_group_service_，回调组，我们前面创建回调组就是在这里使用的，告诉ROS2，当你要调用回调函数处理请求时，请把它放到单独线程的回调组中
```

**7.编写回调函数**

```c++
// 设置rate周期为1s，代表1s检查一次
rclcpp::Rate loop_rate(1);  //循环外
loop_rate.sleep();   //循环内

//判断系统是否还在运行
if(!rclcpp::ok())
{
     RCLCPP_ERROR(this->get_logger(), "程序被终止了");
     return ;
}
```

**8.修改main函数**

要让单线程变为多线程，需要对main函数进行一些修改。

```c++
int main(int argc,char **argv)
{
    rclcpp::init(argc,argv);
    //产生一个节点
    auto node = std::make_shared<SingleDogNode>("节点名");
    //运行节点，并检测退出信号
    rclcpp::executors::MultiThreadedExecutor executor;
    executor.add_node(node);
    executor.spin();
    rclcpp::shutdown();
    return 0;
}
```

**9.测试**

运行之后，可以用   ros2 service  list  -t 查看服务列表

手动发布请求：  ros2 service call  服务  接口  “{发布的数据}”

###2.3.4 客户端实现

**1.创建客户端节点**

创建节点

```ros2
ros2 pkg create village_zhang --build-type ament_cmake --dependencies rclcpp
```

先添加依赖，使得自定义的接口能被找到。

修改 package.xml  ，添加如下代码：

```c++
  <depend>village_interfaces</depend>
```

修改 CMakeLists.txt , 添加如下代码：

```cmake
find_package(village_interfaces REQUIRED)
ament_target_dependencies(zhang3_node
  rclcpp 
  village_interfaces
)
```

**2.创建客户端和请求函数和回调函数**

```c++
// 创建一个客户端
rclcpp::Client<village_interfaces::srv::SellNovel>::SharedPtr client_;
//创建请求函数和回调函数
请求函数；  //自定义，在其中调用回调函数作为接收
回调函数；  
void                                                                 callback(rclcpp::Client<接口名称>::SharedFuture  response)
{
}
```

**3.实例化客户端，编写请求函数和回调函数**

```c++
//实例化客户端
client_ = this->create_client<village_interfaces::srv::SellNovel>("sell_novel");
//编写请求函数，等待服务器上线，构造请求数据，发送异步请求。
 void buy_novel()
 {
     RCLCPP_INFO(this->get_logger(), "买小说去喽");
     //1.等待服务端上线
     while (!client_->wait_for_service(std::chrono::seconds(1)))
      {
          //等待时检测rclcpp的状态
          if (!rclcpp::ok())
          {
               RCLCPP_ERROR(this->get_logger(), "等待服务的过程中被打断...");
               return;
          }
          RCLCPP_INFO(this->get_logger(), "等待服务端上线中");
      }
        
      //2.构造请求的钱
      auto request = std::make_shared<village_interfaces::srv::SellNovel_Request>();
      //先来五块钱的看看好不好看
      request->money = 5; 
      //3.发送异步请求，然后等待返回，返回时调用回调函数
     //此行为的目的是，当我们请求（买书）成功时，调用此回调函数，把结果通过回调函数的参数传递过来。
     client_->async_send_request(request,std::bind(&PoorManNode::novels_callback, this, _1));
};


//编写回调函数处理结果
    //创建接收到小说的回调函数
    void novels_callback(rclcpp::Client<village_interfaces::srv::SellNovel>::SharedFuture  response)
    {
        auto result = response.get();
        
        RCLCPP_INFO(this->get_logger(), "收到%d章的小说，现在开始按章节开读", result->novels.size());
        
        for(std::string novel:result->novels)
        {
            //打印小说章节内容
            RCLCPP_INFO(this->get_logger(), "%s", novel.c_str());
        }
        
        RCLCPP_INFO(this->get_logger(), "小说读完了，好刺激，写的真不错，好期待下面的章节呀！");
    }
```

**4.修改main函数调用请求函数**

```c++
int main(int argc, char **argv)
{
    rclcpp::init(argc, argv);
    /*产生一个Zhang3的节点*/
    auto node = std::make_shared<PoorManNode>();
    node->buy_novel();
    /* 运行节点，并检测rclcpp状态*/
    rclcpp::spin(node);
    rclcpp::shutdown();
    return 0;
}
```

**5.修改CMakeLists.txt**

```c++
//将编译好的可执行文件安装到install目录下
install(TARGETS
  zhang3_node
  DESTINATION lib/${PROJECT_NAME}
)
```



## 2.4 参数介绍

参数是一个节点的配置值，我们可以设置参数使得节点动态地发生变化而无须去改变代码。参数是由键值对组成的，键是string类型，而值有 bool,int64,float64,string,byte以及他们的数组几种类型。

### 2.4.1 基本参数操作介绍

**查看节点参数**

运行节点后

```zsh
ros2 param list
```

**详细查看某个参数信息**

 ```c++
 ros2 param describe <node_name> <param_name>
 ```

**查看参数值**

```c++
ros2 param get <node_name> <param_name>
```

**设置参数**

```c++
ros2 param set <node_name> <parameter_name> <value>
```

**保存参数**

```c++
ros2 param dump <node_name>
//保存为了yaml文件
```

**查看保存的参数文件**

```c++
cat <parameter_file>
```

**重开后使用参数文件**

```c++
ros2 param load <node_name> <parameter_file>
```

**启动节点时直接加载参数**

```c++
ros2 run <package_name> <executable_name> --ros-args --params-file <file_name>
```

###2.4.2 参数编写

| 函数名称               | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| **declare_parameter**  | 声明和初始化一个参数                                         |
| **declare_parameters** | 声明和初始化一堆参数                                         |
| **get_parameter**      | 通过参数名字获取一个参数                                     |
| **get_parameters**     | 获取具有给定前缀的所有参数的参数值                           |
| **set_parameters**     | 设置一组参数的值                                             |
| 更多函数               | [rclcpp： rclcpp：Node](https://docs.ros2.org/latest/api/rclpy/api/node.html) |

**声明参数**

```c++
//声明一下书的单价
unsigned int novel_price = 1;
//在构造函数中声明参数\
//声明参数
this->declare_parameter<std::uint32_t>("novel_price", novel_price);
```

**获取并设置参数**

```c++
//在用到参数的地方更新参数
this->get_parameter("novel_price",novel_price);
```



## 2.5 Action

动作 Action 有着三大组成部分：目标，反馈和结果。

目标：Action 客户端告诉服务端要做什么，服务端针对该目标要有响应。（让我们确认服务端已经接收并处理的目标）。

反馈：Action 服务端告诉客户端此时的进度。

结果：Action 服务端告诉客户端其执行结果，结果最后返回，用于表示任务最终执行情况。

注：参数由服务构建，而Action是由话题和服务共同构建的（一个Action = 三个服务+两个话题） 三个服务分别是：1.目标传递服务    2.结果传递服务    3.取消执行服务 两个话题：1.反馈话题（服务发布，客户端订阅）   2.状态话题（服务端发布，客户端订阅）

### 2.5.1 Action 的 CLI 工具

**action list**

```c++
//获取当前系统中的action列表
ros2 action list
//加上 -t，可看到action的类型
ros2 action list -t
//知道了接口类型后，获取接口信息
ros2 interface show  <interface>
例：
ros2 action list -t 
得到 ：/turtle1/rotate_absolute [turtlesim/action/RotateAbsolute]
ros2 interface show turtlesim/action/RotateAbsolute 
得到 ： 
# The desired heading in radians
float32 theta
---
# The angular displacement in radians to the starting position
float32 delta
---
# The remaining rotation in radians
float32 remaining    
```

**action  info**

```c++
//查看action信息，在终端中输入下面的指令。
ros2 action info <action>
//可以看到action客户端和服务段的数量以及名字。
```

**action send_goal**

```c++
//发送actin请求到服务端，返回goal和result
ros2 action send_goal <action>  <interfaces> "数据"
例：
ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.6}"
//外加返回feedback
ros2 action send_goal <action> <interfaces> "数据" --feedback
例：
ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.5}" --feedback
```



#3.ROS2常用工具

## 3.1 launch文件

编写launch文件可以同时运行多个节点。（节点之间存在依赖关系）。

ROS2 中的launch有三种格式，python,xml,yaml，推荐使用 python 。

**编写过程**

**1.创建文件**

在功能包下某个需要运行的节点下创建launch文件夹，在此文件夹中创建    village.launch.py   文件，需要导入两个库，一个叫做  LaunchDescription  ，用于对  launch文件内容进行描述，一个是Node，用于声明节点所在的位置。同时文件中一定要有一个名字叫  generate_launch_description  的函数。

```c++
# 导入库
from launch import LaunchDescription
from launch_ros.actions import Node

# 定义函数名称为：generate_launch_description
def generate_launch_description():
    # 创建Actions.Node对象，标明第一个启动节点所在位置
    可执行文件名 = Node(
        package="功能包名",
        executable="可执行文件"
        )
    # 创建Actions.Node对象，标明第二个启动节点所在位置
    可执行文件名 = Node(
        package="功能包名",
        executable="可执行文件名"
        )
    # 创建LaunchDescription对象launch_description,用于描述launch文件
    launch_description = LaunchDescription([可执行文件1,可执行文件2])
    # 返回让ROS2根据launch描述执行节点
    return launch_description
        
        
 //举例子
 # 导入库
from launch import LaunchDescription
from launch_ros.actions import Node

# 定义函数名称为：generate_launch_description
def generate_launch_description():
    # 创建Actions.Node对象li_node，标明李四所在位置
    li4_node = Node(
        package="village_li",
        executable="li4_node"
        )
    # 创建Actions.Node对象wang2_node，标明王二所在位置
    wang2_node = Node(
        package="village_wang",
        executable="wang2_node"
        )
    # 创建LaunchDescription对象launch_description,用于描述launch文件
    launch_description = LaunchDescription([li4_node,wang2_node])
    # 返回让ROS2根据launch描述执行节点
    return launch_description
```

**2.cmake编译文件**

```c++
//在CMakeLists文件中添加以下指令，将launch文件夹安装到install目录下
 
```

**3.编译运行**

```
colcon build
ros2 launch village_wang village.launch.py 
```

**4.添加参数**

很简单，直接在创建的对象中添加即可。

```c++
# 导入库
from launch import LaunchDescription
from launch_ros.actions import Node

# 定义函数名称为：generate_launch_description
def generate_launch_description():
    # 创建Actions.Node对象li_node，标明李四所在位置
    li4_node = Node(
        package="village_li",
        executable="li4_node",
        output='screen',  #四个可选项 
        parameters=[{'write_timer_period': 1}]
        )
    # 创建Actions.Node对象wang2_node，标明王二所在位置
    wang2_node = Node(
        package="village_wang",
        executable="wang2_node",
        output='screen',  #四个可选项 
        parameters=[{'novel_price': 2}]
        )
    # 创建LaunchDescription对象launch_description,用于描述launch文件
    launch_description = LaunchDescription([li4_node,wang2_node])
    # 返回让ROS2根据launch描述执行节点
    return launch_description
```



## 3.2 rosbag2 

用于记录话题数据并实现重放。相当于继续发布话题。

**常用指令**

**记录某个话题**

```bash
ros2 bag record topic-name
```

**记录多个话题的数据**

  ```c++
  ros2 bag record topic-name1  topic-name2
  ```

**记录所有话题**

```bash
ros2 bag record -a
```

**自定义输出文件的名字**

```bash
ros2 bag record -o file-name topic-name
```

**查看录制的话题的信息**

```bash
ros2 bag info bag-file
```

**播放**

```bash
ros2 bag play xxx.db3
```

**查看播放**

```bash
ros2 topic echo topic-name
```

**倍速播放**

-r选项可以修改播放速率，比如 -r 值，比如 -r 10,就是10倍速，十倍速播放话题

-l 循环播放

```bash
ros2 bag play bag-file-name -r n
```

**单独播放某个话题**

```bash
ros2 bag play bag-file-name --topics  topic-name
```



##3.3 RQT工具

### 3.3.1 简介

RQT是一个GUI框架，通过==插件的方式==实现了各种各样的界面工具。

### 3.3.2 插件

1.   `Introspection/ Node Graph`              qt_graph,插件名字叫做Node Graph，这个**插件用于查看节点和节点之间的关系的**。
2.  'Introspection / Process Monitor'       这个插件可以看到所有与ROS2相关的进程
3. Topic / Message Publisher       可以图形化发布话题数据
4. Service / Service Caller    图形化调用服务工具
5.  Visualization / Image  View   看图像话题数据的Image View
6. Visualization / MatPlot   话题数据图形化工具MqtPlot
7. Configuration / Parameter Reconfigure  



## 3.4 RVIZ2

### 3.4.1 简介

RVIZ2是ROS2中的一个==非常重要且常用的数据可视化工具==。

- 数据：各种调试机器人时常用的数据，比如：图像数据、三维点云数据、地图数据、TF数据，机器人模型数据等等
- 可视化：可视化就是让你直观的看到数据，比如说一个三维的点(100,100,100),通过RVIZ可以将其显示在空间中
- 如何做到不同数据的可视化：强大的插件，如果没有你的数据，你可以自己再写一个插件，即插即用，方便快捷

###3.4.2 基础配置

**全局配置**

- Fixed Frame：所有帧参考的帧的名称，坐标都是相对的，这个就是告诉RVIZ你是相对谁的，一般是设置成map或者odom。
- Frame Rate：用于设置更新 3D 视图的最大频率。

**网格**

用于可视化通常与地板平面相关联的网格。

- Reference frame：帧用作网格坐标参考（通常：）
- Plane cell count: 单元格中网格的大小
- Normal cell count：在沿垂直于叶栅平面的网格数（正常：0）
- Cell size：每个网格单元的尺寸（以米为单位）
- Plane：标识网格平面的两个轴

**机器人模型**

根据 URDF 模型的描述来可视化机器人的模型。

- Visual enabled: 启用/禁用模型的 3D 可视化
- Description Source：机器人模型文件的来源，可以在File和Topic之间进行选择
- Description Topic: 机器人模型文件所在的话题

**TF**

可视化构成 TF 广播的所有帧的位置和方向。

- Marker Scale: 将字和坐标系标识调整的小一些，使其更加可见且不那么混乱
- Update interval：以秒为单位的TF广播更新时间



## 3.5 Gazebo介绍

### 3.5.1 简介

**RVIZ2是用来可视化数据的软件，核心要义是将数据展示出来（我们不生产数据只做数据的搬运工）。 而Gazebo是用于模拟真实环境生产数据的（我们不搬运数据只做数据的生产者）**

Gazebo可以根据我们所提供的机器人模型文件，传感器配置参数，给机器人创造一个虚拟的环境，虚拟的电机和虚拟的传感器，并通过ROS/ROS2的相关功能包把传感器数据电机数据等发送出来。

### 3.5.2 集成

**Gazebo 是一个独立的应用程序，可以独立于 ROS 或 ROS 2 使用。** 

Gazebo与ROS 版本的集成是通过一组叫做`gazebo_ros_pkgs`的包 完成的，`gazebo_ros_pkgs`将Gazebo和ROS2连接起来。

###3.5.3  **gazebo_ros_pkgs**

gazebo_ros_pkgs不是一个包，是一堆包如下：

- gazebo_dev：开发Gazebo插件可以用的API
- gazebo_msgs：定义的ROS2和Gazebo之间的接口（Topic/Service/Action）
- gazebo_ros：提供方便的 C++ 类和函数，可供其他插件使用，例如转换和测试实用程序。它还提供了一些通常有用的插件。gazebo_ros::Node
- gazebo_plugins：一系列 Gazebo 插件，将传感器和其他功能暴露给 ROS2  例如:
  1. `gazebo_ros_camera` 发布ROS2图像
  2. `gazebo_ros_diff_drive` 通过ROS2控制和获取两轮驱动机器人的接口



# 4.ROS2链接自己的库

[基础操作](https://zhuanlan.zhihu.com/p/437550838)

[高级操作](https://zhuanlan.zhihu.com/p/438191834)

## 4.1 添加库

```cmake
# 添加源文件，生成库
add_library(包名（common） SHARED
        头文件    （include/common/common.h）
        源文件     （src/common.cpp）
        )
# 添加头文件地址
target_include_directories(包名（my_math_lib） PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
        )
```

##4.2 链接依赖

```cmake
# 用于代替传统的target_link_libraries
ament_target_dependencies(包名（common）
        PUBLIC
        依赖 (rclcpp)
        )
```

## 4.3 导出依赖

```cmake
# 为了使下游文件可以访问
ament_export_targets(包名(common) HAS_LIBRARY_TARGET)
ament_export_dependencies(
        依赖(rclcpp)
)

# 注册 导出头文件
install(
        DIRECTORY include/
        DESTINATION include
)


# 注册 导出库文件
install(
        TARGETS 包名(common) # 告诉ros2有这么个目标（可执行文件或者库）
        EXPORT 包名(common)
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
        INCLUDES DESTINATION include
)
ament_package()
```

##4.4 使用

就跟正常的库一样（如rclcpp）。

#5.launch文件详解

```python
# 导入库
from launch import LaunchDescription
from launch_ros.actions import Node

# 定义函数名称为：generate_launch_description
def generate_launch_description():
    # 创建Actions.Node对象，标明第一个启动节点所在位置
    可执行文件名 = Node(
        package="功能包名",
        executable="可执行文件"
        )
    # 创建Actions.Node对象，标明第二个启动节点所在位置
    可执行文件名 = Node(
        package="功能包名",
        executable="可执行文件名"
        )
    # 创建LaunchDescription对象launch_description,用于描述launch文件
    launch_description = LaunchDescription([可执行文件1,可执行文件2])
    # 返回让ROS2根据launch描述执行节点
    return launch_description
```

##5.1 常用API

###5.1.1  generate_launch_description()

`launch`文件中定义了一个`generate_launch_description()` 函数，它返回`LaunchDescription` 。

###5.1.2 LaunchDescription

`LaunchDescription` 中写了需要启动的节点，当然还可以加其他的内容，比如参数声明等等。

###5.1.3 output='screen'

`output='screen'` 的意思是执行的节点打印的log直接在窗口中输出。

### 5.1.4获取路径：get_package_share_directory

```python
import os
from ament_index_python.packages import get_package_share_directory
# Get the launch directory
bringup_dir = get_package_share_directory('nav2_bringup')#获取包的路径
launch_dir = os.path.join(bringup_dir, 'launch')#组合包内路径
```

### 5.1.5 设置环境变量：SetEnvironmentVariable

```python
from launch.actions import SetEnvironmentVariable
stdout_linebuf_envvar = SetEnvironmentVariable('RCUTILS_LOGGING_BUFFERED_STREAM', '1')
```

### 5.1.6 **获取环境变量的值**:os.environ

```python
import os
TURTLEBOT3_MODEL = os.environ['TURTLEBOT3_MODEL']
```

### 5.1.7 **包含另外一个python launch文件**

```py
from launch.actions import IncludeLaunchDescription
from launch.launch_description_sources import PythonLaunchDescriptionSource

IncludeLaunchDescription(
    PythonLaunchDescriptionSource([nav2_launch_file_dir, '/bringup_launch.py']),
    launch_arguments={
        'map': map_dir,
        'use_sim_time': use_sim_time,
        'params_file': param_dir}.items(), #这里给内嵌的python launch文件传入参数
),
```

### 5.1.8 **给一组节点添加同一个命名空间**

```python
from launch.actions import GroupAction
from launch_ros.actions import PushRosNamespace

   ...
   turtlesim_world_2 = IncludeLaunchDescription(
      PythonLaunchDescriptionSource([os.path.join(
         get_package_share_directory('launch_tutorial'), 'launch'),
         '/turtlesim_world_2.launch.py'])
      )
   turtlesim_world_2_with_namespace = GroupAction(
     actions=[
         PushRosNamespace('turtlesim2'),
         turtlesim_world_2,
      ]
   )
```



## 5.2 整体布局

###5.2.1 导入模块

这部分其实是python的语法。意思是，如果你想用另一个python文件的函数，需要将其import进来，类似于C/C++中的include语法。比如下面的写法：

```python
from launch import LaunchDescription
import launch_ros.actions
```

###5.2.2 参数声明

这一部分主要是声明有哪些参数，并且给每个参数赋上默认值。

**声明参数:`LaunchConfiguration`**

在launch系统中，以名称来访问参数的值,上级`launch`文件可以传值给下级`launch`文件。

```python
# Input parameters declaration
namespace = LaunchConfiguration('namespace')
params_file = LaunchConfiguration('params_file')
use_sim_time = LaunchConfiguration('use_sim_time')
#如果launch系统中没有这个参数，可以给它赋默认值
use_sim_time = LaunchConfiguration('use_sim_time', default='false')
autostart = LaunchConfiguration('autostart')
```

**给参数赋默认值:`DeclareLaunchArgument`**

这些参数是上级的launch文件传给下级的launch文件的参数。

```python
    # Declare the launch arguments
    declare_namespace_cmd = DeclareLaunchArgument(
        'namespace',
        default_value='',
        description='Top-level namespace')

    declare_params_file_cmd = DeclareLaunchArgument(
        'params_file',
        default_value=os.path.join(bringup_dir, 'params', 'nav2_params.yaml'),
        description='Full path to the ROS2 parameters file to use for all launched nodes')

    declare_use_sim_time_cmd = DeclareLaunchArgument(
        'use_sim_time',
        default_value='True',
        description='Use simulation (Gazebo) clock if true')

    declare_autostart_cmd = DeclareLaunchArgument(
        'autostart', default_value='True',
        description='Automatically startup the nav2 stack')
```

参数配置好后就可以输入到node节点中了。同时，这些参数也可以在==启动launch文件的同时临时修改==。

```python
ros2 launch <package_name> <launch_file_name> use_sim_time:=0
ros2 launch launch_tutorial example_substitutions.launch.py turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```

### 5.2.3 构建需启动的节点

这个部分主要是写明需要启动哪些节点。并且将节点需要的参数配置好。

```python
 # Nodes launching commands

    start_map_saver_server_cmd = Node(
            #在哪个包里
            package='nav2_map_server',
            #运行哪个可执行文件
            executable='map_saver_server',
            #输出在输出端
            output='screen',
            #设置参数
            parameters=[configured_params])
        
    start_lifecycle_manager_cmd = Node(
            package='nav2_lifecycle_manager',
            executable='lifecycle_manager',
            name='lifecycle_manager_slam',
            output='screen',
            parameters=[{'use_sim_time': use_sim_time},
                        {'autostart': autostart},
                        {'node_names': lifecycle_nodes}])
                        
    start_rviz_cmd = Node(
            package='rviz2',
            executable='rviz2',
            name='rviz2',
            arguments=['-d', rviz_config_dir],
            parameters=[{'use_sim_time': use_sim_time}],
            output='screen')
            
    # perform remap so both turtles listen to the same command topic
    forward_turtlesim_commands_to_second_turtlesim_node = Node(
            package='turtlesim',
            executable='mimic',
            name='mimic',
           #将两个话题名称对等。左边是节点内部代码内写明的，右边为系统中其他节点提供的话题。
            remappings=[
                ('/input/pose', '/turtlesim1/turtle1/pose'),
                ('/output/cmd_vel', '/turtlesim2/turtle1/cmd_vel'),
            ]
        )
```

`parameters` 中可以配置`node`的参数。这些参数通常会在参数声明部分赋好值，这里直接传给节点即可。同时这里也可以直接传入`yaml`**参数文件**。`arguments=['-d', rviz_config_dir]` 是节点内部实现的参数。通常通过`main`函数的行参获取。

==给节点配置yaml参数文件的示例==：

```python
import os

from ament_index_python.packages import get_package_share_directory

from launch import LaunchDescription
from launch_ros.actions import Node


def generate_launch_description():
   config = os.path.join(
      get_package_share_directory('launch_tutorial'),
      'config',
      'turtlesim.yaml'
      )

   return LaunchDescription([
      Node(
         package='turtlesim',
         executable='turtlesim_node',
         namespace='turtlesim2',
         name='sim',
         parameters=[config]
      )
   ])
```

**启动命令**

这一部分只需将前面配置好的命令，用`add_action`方法加入即可。当然，这里其实有**两种写法**。一种是下面这种。

```python
# Create the launch description and populate
ld = LaunchDescription()

# Set environment variables
ld.add_action(stdout_linebuf_envvar)

# Declare the launch options
ld.add_action(declare_namespace_cmd)
ld.add_action(declare_use_namespace_cmd)
ld.add_action(declare_slam_cmd)
ld.add_action(declare_map_yaml_cmd)
ld.add_action(declare_use_sim_time_cmd)
ld.add_action(declare_params_file_cmd)
ld.add_action(declare_autostart_cmd)

# Add the actions to launch all of the navigation nodes
ld.add_action(bringup_cmd_group)

return ld
```

还有一种是：

```python
return LaunchDescription([
    DeclareLaunchArgument(
        'map',
        default_value=map_dir,
        description='Full path to map file to load'),

    DeclareLaunchArgument(
        'params_file',
        default_value=param_dir,
        description='Full path to param file to load'),

    DeclareLaunchArgument(
        'use_sim_time',
        default_value='false',
        description='Use simulation (Gazebo) clock if true'),

    IncludeLaunchDescription(
        PythonLaunchDescriptionSource([nav2_launch_file_dir, '/bringup_launch.py']),
        launch_arguments={
            'map': map_dir,
            'use_sim_time': use_sim_time,
            'params_file': param_dir}.items(),
    ),

    Node(
        package='rviz2',
        executable='rviz2',
        name='rviz2',
        arguments=['-d', rviz_config_dir],
        parameters=[{'use_sim_time': use_sim_time}],
        output='screen'),
])
```





# 6.相机封装小结

为了解决图像零拷贝赋值问题，需要用到容器，我们把节点改为构件放入其中来达到此目的。

[component参考教程古月居](https://www.guyuehome.com/38535)

[intra_process官方文档](https://docs.ros.org/en/foxy/Tutorials/Demos/Intra-Process-Communication.html)

[intra_process官方代码](https://github.com/ros2/demos/blob/foxy/intra_process_demo/src/two_node_pipeline/two_node_pipeline.cpp)

[component官方代码](https://github.com/ros2/demos/tree/foxy/composition)

[component官方文档](https://docs.ros.org/en/foxy/Tutorials/Intermediate/Composition.html)

[容器背景概念官方文档](https://docs.ros.org/en/foxy/Concepts/About-Composition.html)



## 6.1 容器梳理

把节点变为构件，在launch文件中调用ros2自带的rclcpp_component容器。

###6.1.1 hpp

首先是hpp文件，主要就是文件开头的宏定义

```c++
#ifndef COMPOSITION__TALKER_COMPONENT_HPP_
#define COMPOSITION__TALKER_COMPONENT_HPP_
//放头文件
//#include "rclcpp/rclcpp.hpp"
//#include "std_msgs/msg/string.hpp"
//#include <chrono>
//#include <iostream>
//#include <memory>
//#include <utility>

namespace composition
{

class Talker : public rclcpp::Node
{
public:
//暂不知此宏定义的含义,猜测跟options有关
//   COMPOSITION_PUBLIC
  explicit Talker(const rclcpp::NodeOptions & options);

protected:
  void on_timer();

private:
  size_t count_;
  rclcpp::Publisher<std_msgs::msg::String>::SharedPtr pub_;
  rclcpp::TimerBase::SharedPtr timer_;
};

}  // namespace composition

#endif  // COMPOSITION__TALKER_COMPONENT_HPP_

```

官方代码中对上述的宏定义  COMPOSITION_PUBLIC 采用了如下定义，但我加入后报错，问题未知。所以暂时没有加。

```c++
//#ifdef __GNUC__
//     #define COMPOSITION_EXPORT __attribute__ ((dllexport))
//     #define COMPOSITION_IMPORT __attribute__ ((dllimport))
//   #else
//     #define COMPOSITION_EXPORT __declspec(dllexport)
//     #define COMPOSITION_IMPORT __declspec(dllimport)
//   #endif
//   #ifdef COMPOSITION_BUILDING_DLL
//     #define COMPOSITION_PUBLIC COMPOSITION_EXPORT
//   #else
//     #define COMPOSITION_PUBLIC COMPOSITION_IMPORT
//   #endif

// #ifdef COMPOSITION_BUILDING_DLL
//     #define COMPOSITION_PUBLIC COMPOSITION_EXPORT
// #else
//     #define COMPOSITION_PUBLIC COMPOSITION_IMPORT
// #endif

// #include "composition/visibility_control.h"x
```

### 6.1.2 cpp

重点在于文件末尾的注册函数


```c++
#include "composition_test/talker_component.hpp"
using namespace std::chrono_literals;

namespace composition
{
Talker::Talker(const rclcpp::NodeOptions & options)
: Node("talker", options), count_(0)
{
  // Create a publisher of "std_mgs/String" messages on the "chatter" topic.
  pub_ = create_publisher<std_msgs::msg::String>("chatter", 10);

  // Use a timer to schedule periodic message publishing.
  timer_ = create_wall_timer(1s, std::bind(&Talker::on_timer, this));
}

void Talker::on_timer()
{
  auto msg = std::make_unique<std_msgs::msg::String>();
  msg->data = "Hello World: " + std::to_string(++count_);
  RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", msg->data.c_str());
  std::flush(std::cout);

  // Put the message into a queue to be processed by the middleware.
  // This call is non-blocking.
  pub_->publish(std::move(msg));
}

}  // namespace composition

#include "rclcpp_components/register_node_macro.hpp"

// Register the component with class_loader.
// This acts as a sort of entry point, allowing the component to be discoverable when its library
// is being loaded into a running process.
RCLCPP_COMPONENTS_REGISTER_NODE(composition::Talker)
```

==注：==cpp里面不要写main函数。

### 6.1.3 cmake

1.首先要加依赖，特别的容器的依赖为 `find_package(rclcpp_components REQUIRED)`

2.添加自己写的头文件`include_directories(include)`

3.将构件编译为动态链接库使得其他文件能够调用。

```cmake
add_library(
   talker_component SHARED
   src/talker_component.cpp
)
```

4.把依赖添加到构件上

```c++
ament_target_dependencies(
  talker_component
  "rclcpp"
  "rclcpp_components"
)
```

5.把构件放到自己的install目录下，这样运行的时候才找得到

```cmake
install(TARGETS
  talker_component
  listener_component
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)
```

6.注册自己的构件

```cmake
rclcpp_components_register_nodes(talker_component "composition::Talker")
rclcpp_components_register_nodes(listener_component "composition::Listener")
```

### 6.1.4 xml文件

添加一个` <depend>rclcpp_components</depend>`即可。

### 6.1.5 launch文件

在文件中可以手动调用容器，向其中加入构件实现一键启动。

```python
import launch
from launch_ros.actions import ComposableNodeContainer
from launch_ros.descriptions import ComposableNode

def generate_launch_description():
    container = ComposableNodeContainer(
        #容器直接用ros2自带的容器即可
            name='my_container',
            namespace='',
            package='rclcpp_components',
            executable='component_container',
            #向容器加入各个构件
            composable_node_descriptions=[
                ComposableNode(
                    package='ros2_usbcapture',
                    plugin='wmj::camera_node',
                    name='talker',
                    #各个构建要求参数options，所以可以在这里直接定义参数，比如这里的开启的intra_process
                    extra_arguments=[{'use_intra_process_comms': True}]
                    ),
                ComposableNode(
                    package='ros2_usbcapture',
                    plugin='wmj::Image_get_test',
                    name='listener',
                    extra_arguments=[{'use_intra_process_comms': True}]
                    )
            ],
            output='screen',
    )

    return launch.LaunchDescription([container])

```

## 6.2 cv_bridge

ros2传输需要用到自己的sensor_msgs下的Image接口，但是我们读图用的是cv::mat，需要用到cv_bridge包下的cvImage来进行转换。

```c++
//CvImage源码
namespace cv_bridge {
class CvImage
{
public:
std_msgs::Header header;
std::string encoding;
cv::Mat image;
};
```

###6.2.1 将ros信息转换为opencv

v_bridge将ROS的图像消息转换为OpenCV图像格式时都是通过CvImage类实现的。一般来说，cv_bridge提供了两种方式转换为CvImage类，分别为复制（copy）和共享（share）。

```c++
// Case 1: Always copy, returning a mutable(可变的) CvImage
CvImagePtr toCvCopy(const sensor_msgs::ImageConstPtr& source,
const std::string& encoding = std::string());
CvImagePtr toCvCopy(const sensor_msgs::Image& source,
const std::string& encoding = std::string());

// Case 2: Share if possible, returning a const CvImage
CvImageConstPtr toCvShare(const sensor_msgs::ImageConstPtr& source,
const std::string& encoding = std::string());
CvImageConstPtr toCvShare(const sensor_msgs::Image& source,
const boost::shared_ptr& tracked_object,
const std::string& encoding = std::string());
```

1）toCvCopy函数会从ROS消息中拷贝一个图像数据，不管如何修改CvImage类的内容都不会影响源数据，使用例子如下。

```c++
cv::Mat img;
cv_bridge::CvImagePtr cv_ptr;
cv_ptr = cv_bridge::toCvCopy(msg, “mono8”);
cv_ptr->image.copyTo(img);
```

（2）toCvShare函数则是在源和编码都匹配的情况下将返回的OpenCV指针指向ROS的消息，即共享指针地址。它的特点是只要你还保持着返回的CvImage类的副本，那么ROS的消息类型就不会被释放。当编码参数不匹配时，它还能够另外开辟一个新的缓存区并执行编码参数转换工作。toCvShare函数对于有一个指向另一个相同类型消息的指针时会很方便。如果没有给定编码类型时，toCvShare会使目标图像编码将会与消息编码相同。toCvShare函数的使用例子如下。

```c++
cv::Mat img;
cv_bridge::CvImageConstPtr cv_ptr;
cv_ptr=cv_bridge::toCvShare(msg, “mono8”);
img = cv_ptr->image;
```
###6.2.2 将opencv转换为ros

在CvImage类中执行的OpenCV图像转换为ROS消息的成员函数为 toImageMsg()，其源码中的定义如下所示：

```c++
class CvImage
{
sensor_msgs::ImagePtr toImageMsg() const;
// Overload mainly intended for aggregate messages that contain
// a sensor_msgs::Image as a member.
void toImageMsg(sensor_msgs::Image& ros_image) const;
};
```

该函数的输入有三个：标准消息包的头、图像编码和OpenCV源图像，使用例程如下：

```c++
cv::Mat image = cv::imread(…);
sensor_msgs::ImagePtr msg = cv_bridge::CvImage(std_msgs::Header(), “bgr8”, image).toImageMsg();
```





















