# CMAKE

###1.基本语法

#### 1.1  指令（参数1   参书2）

参数用括弧括起来，参数之间用**空格或者分号**分隔。

**指令不区分大小写，而参数要区分大小写**。

变量使用 { } 方式取值， 但在 IF 语句中直接使用变量名。

### 2.重要指令

#### 2.1  ==cmake_minimun_required(VERSION  n)==

指定cmake的最小版本要求。

#### 2.2  ==project(  名字  )==

给工程命名。

#### 2.3  set (   )

显式的定义变量。

#### 2.4 ==include_directories(   )==

向工程添加多个指定的头文件搜索路径。

#### 2.5  link_directories(  )

向文件添加指定的库文件搜索路径。

#### 2.6 add_library(  )

添加库文件

#### 2.7 add_compile_options

添加参数编译

#### 2.8 ==add_executable(可执行文件名称    source1  source2 …)==

生成可执行文件

#### 2.9 ==target_link_libraries(目标文件     库1  库2….  )==

为  target 添加需要链接的共享库。

#### 2.10 add_subdirectory(  )

向当前工程添加存放源文件的子目录，并可以指定中间二进制和目标二进制存放的位置。

#### 2.11 add_source_directory

发现一个目录下的所有源代码文件并将列表储存在一个变量中，这个指令临时被用来自动构建源文件

###3.常用变量

#### 3.1 CMAK_C_FLAGS

gcc编译选项

#### 3.2  CMAKE_CXX_FLAGS

g++编译选项

#### 3.3  CMAKE_BUILD_TYPE

编译类型，有 Debug(调试)  和  Release(发布) 两种选项

例：  set(CMAKE_BUILD_TYPE   Debug)

#### 3.4  CMAKE_C_COMPILER   /  CMAKE_CXX_COMPILER

指定C编译器

#### 3.5  EXECUTABLE_OUTPUT_PATH

可执行文件的存放路径

#### 3.6  LIBRARY_OUTPUT_PATH

库文件输出的存放路径

### 4.编译工程

#### 4.1 CMake目录结构

1.项目主目录存在一个CMakeLists.txt文件。

2.包含源文件的子文件夹包含 CMakeLists.txt 文件，主目录的CMakeLists.txt通过 add_subdirectory 添加子目录。

3.包含源文件的子文件夹未包含CMakeLists.txt文件，子目录编译规则体现在主目录的CMakeLists.txt中。

#### 4.2编译流程

1.手动编写CMakeLists.txt文件。

2.执行命令cmake PATH 生成Makefile（PATH是顶层CMakeLists.txt所在目录）。

3.执行命令make进行编译。

#### 4.3两种构建方式

##### 4.3.1内部构建

直接在当前目录下，用cmake编译，再执行make.

##### 4.3.2外部构建

在当前目录下创建build文件夹，进入该文件夹，编译上一级目录的文件，再执行make 命令。