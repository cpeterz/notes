# WMJ代码梳理

## 1.model  识别模型库

### 1.1 使用方法

1.master 分支为空白分支，请在这里更新版本号；除 master 以外的所有分支都将被设为只读分支，不允许进行推送

2.如果需要更新模型，请从 master 分支新建新分支。所有分支以版本号命名，如 v1.0

3.在 Armor 仓库的 CMakeLists.txt 中声明了所使用到的 model 版本，需与该仓库的版本（分支名）对应，否则 CMake 将会报错

###1.2 版本号说明

| 版本号 | 说明         | 时间     |
| ------ | ------------ | -------- |
| v1.x   | 传统视觉模型 |          |
| v1.0   | 最初模型     | 2022.7.6 |
| v1.1   |              | 2022.7.6 |





## 2.Armor装甲板识别类

### 2.1 识别流程

#### 2.1.1 双目

![img](https://s3.bmp.ovh/imgs/2021/08/09e89be69600562d.png)

根据相机数目判断读入的图像数目，根据图像数目决定是使用双目还是单目。

#### 2.1.2 单目

![img](https://s3.bmp.ovh/imgs/2021/08/1aad18163837dfcf.png)

经过一系列的图像处理来找到装甲板。

## 3. Pose  位姿解算类

### 3.1 主要接口

```c++
wmj::GimbalPose wmj::AngleSolver::getAngle(wmj::Armor armor, wmj::GimbalPose cur_pose, double bullet_speed)
```

参数：

- armor: 目标装甲板
- cur_pose: 当前云台位姿位姿
- bullet_speed: 当前射速

返回：

​     目标云台位姿



```c++
cv::Point3f wmj::AngleSolver::cam2abs(cv::Point3f position, wmj::GimbalPose cur_pose)
```

参数：

- position: 相机系三维坐标
- cur_pose: 当前云台位姿

### 3.2 工作原理

1.通过平移矩阵将相机系相对坐标转换为枪口系相对坐标。

2.根据枪口系相对坐标解算欧拉角。

3.将当前位姿加上相对欧拉角得到目标位姿。

4.根据当前射速计算弹道补偿，修正pitch值。

### 3.3 调参方法

1.从机械图纸得到相机系到枪口系的平移矩阵

2.先关闭弹道补偿，根据激光红点位置微调pitch_off和yaw_off，在不同距离都能将红点对准装甲板中心。

3.打开弹道补偿，在不同距离不同高度不同射速下测试子弹的命中位置。



## 4.Select  装甲板筛选器库

### 4.1 作用

输入识别装甲板结果后输出并返回ROI。

 NORMAL模式：

适用于线性预测自瞄。返回全车大小ROI，粗略筛选。

TOP模式：

适用于反陀螺模式，相较于NORMAL模式返回主要目标的同时返回次要目标。

SENTRY模式：

适用于哨兵模式，只返回哨兵装甲板。

ArmorSelector.yaml—————— 装甲板筛选器参数文件。



## 5. TopAimer  小陀螺自瞄

### 5.1 调用方式

AngleAimer为基于角度的陀螺自瞄，StaticAimer为静态陀螺自瞄，两者继承自Aimer基类。使用一个指向基类的shared_ptr，根据需要实例化对应的类。

主线成循环调用：

```c++
wmj::Armor wmj::Aimer::getFinalPose(wmj::GimbalPose &cur_pose, wmj::Armor &armor, float bullet_speed)
```

参数：

cur_pose: 当前云台位姿

armor: 识别到的装甲板

bullet_speed: 当前射速

返回：

目标位姿

```c++
bool wmj::Aimer::shootable()
```

- 返回：

  射击许可

### 5.2 调试方式

#### 5.2.1 角度

1.以当前状态机帧率和目标估计转速大致确认单个周期的记录帧数，一般以超过20为优，否则模型计算可能误差偏大。

2.观察模型各数据输出，一般情况下基本稳定，则模型参数基本不需要调节。若每个周期击发子弹数量过少，可以适当增大角速度波动阈值、中心漂移和距离比例阈值以及最大击发窗口角度。

3.若命中点位存在偏移，微调`time_off`和`angle_off`

#### 5.2.2 静态

1.一块装甲板从视野中出现到消失的最少帧数限制为`m_min_vec_size`

2.通过水平距离差`horizontal_diff_max`判定是否切换到一块新装甲板

3.瞄准的基础是两块装甲板距离相机最近的位置，在这两个位置的基础上左右做一个线性的预测。目前是认为装甲板在最近位置附近近似做一个匀速直线运动，效果还行。日后会改为用参数方程。

### 5.3 原理

![img](/home/peter/picture/TopAimer.png)



## 6.predict  预测

### 6.1 类属性

```c++
  public:
        enum FILTER_MODE{IDLE, POINT, POSE, SINGLE}; // 滤波器模式
        FILTER_MODE m_filter_mod;					 // 当前滤波器模式
        int         m_measure_num;					 // 测量量维度
        int         m_state_num;					 // 状态量维度

        int         m_init_count_threshold;			 // 初始化阈值
        double       m_process_noise_cov;			 // 噪声协方差
        double       m_measure_noise_cov;			 // 测量协方差
        double        m_control_freq;					 // 控制频率 用于计算dt
        double      m_predict_coe;				 	 // 预测量增益时间 用于增大预测量
        cv::Mat     m_measurement;					 // 观测量矩阵
        std::shared_ptr<cv::KalmanFilter> m_KF; 	 // 滤波器

		// 三维
        cv::Point3f m_last_position;  				 // 上一帧坐标
        double        m_last_armor_dis;				 // 上一帧距离
        double        m_cur_armor_dis;				 // 当前帧距离
        double       m_cur_max_shoot_speed = 15.0;	 // 当前射速上限 不用实时射速避免预测量抖动
        double       m_target_change_dist_threshold;  // 目标切换阈值 距离
        
		// 二维
        wmj::GimbalPose  m_last_pose;					 // 上一帧位姿
        double            m_target_change_pose_threshold; // 目标切换阈值 位姿
        
		// 一维
        double        m_last_value; // 上一帧值
        double     m_value_diff; // 目标切换阈值 变量

    private:
        bool        m_initialized{false}; // 滤波器初始化标志
        int         m_debug;			  // 1为调试模式,非bool
```

### 6.2 主要接口

```c++
Predict::Predict(wmj::Predict::FILTER_MODE a_filter_mode)
```

作用：

构造函数

返回：

- a_filter_mode=POINT: 创建三维点预测器
- a_filter_mode=POSE: 创建二维点预测器
- a_filter_mode=SINGLE: 创建一维点预测器

```c++
void Predict::setParam()
```

作用：

- 预测器的参数初始化，默认读取Predict.yaml中的参数
- 将滤波器状态恢复为未初始化
- 可以在创建滤波器后直接访问类变量进行参数修改（有修改需要的变量已经全部公有了）

```c++
void Predict::initFilter(cv::Point3f a_position)
void Predict::initFilter(wmj::GimbalPose a_pose)
void Predict::initFilter(double a_value)
```

作用：

三种预测器的滤波器初始化，创建时或目标切换时重新初始化使滤波器第一帧收敛

```c++
cv::Point3f Predict::predict(cv::Point3f a_position)  
wmj::GimbalPose Predict::predict(wmj::GimbalPose a_pose)
float Predict::predict(double a_value)
```

作用：

- 预测主函数

参数：

- 对应滤波器的测量量

返回：

- 预测结果（三维点坐标/云台位姿/一维量）

### 6.3  主要流程

1.   初始化参数，根据测距精度(7m误差<10cm)，设置过程噪声`m_processNoiseCov`较小(1e-5)，测量噪声`m_measerementNoiseCov`适中(1e-3)；从参数文件`Predict.yaml`读取`m_init_count_threshold`第一帧预测次数，`m_predict_coe`预测数度增益量，`m_control_freq`控制频率；外部调用`setShootSpeed(float)`设置当前射速上限。

2.   外部直接调用`predict()`获得预测后的装甲坐标，内部判断调用`initFilter()`初始化卡尔曼滤波器，并用第一帧输入的装甲绝对坐标进行多次预测使其收敛

3.    判断当前帧和上一帧装甲相对距离，根据设置的阈值(target_change_threshold=0.25m)判断装甲板是否切换，装甲板切换需重新初始化滤波器

4. ​    得到预测结果，击打远距离装甲板子弹飞行时间更长，不同射速等级子弹飞行时间不同，所以加入装甲距离和射速调节预测量大小。

### 6.4 调参方法

1.  测距精度达到要求，先调试PID参数，完成后开始调试预测。

2.  提前测算自瞄循环的帧率，主要调试predict_coe预测量和control_freq控制频率。

3.  对于较快速的线性运动，有时需要增大target_change_threshold避免滤波器重复初始化造成跟随过程中的抖动，但该阈值过大容易在装甲板切换时预测出错



## 7.PID调节器与速度解算器

### 7.1 主要接口

```c++
wmj::GimbalPose wmj::SpeedResolver::resolve(wmj::GimbalPose target_pose, wmj::GimbalPose cur_pose, double cur_time)
```

参数：

- target_pose: 根据识别结果解算的目标位姿
- cur_pose: 根据陀螺仪回传的云台当前位姿
- cur_time: 当前时间

返回：

​	云台速度

```c++
wmj::GimbalPose wmj::PidResolver::resolve(wmj::GimbalPose cur_error)
```

参数：

- cur_error: 当前误差

返回：

​	速度修正值

### 7.2 工作原理

1.  将目标位姿和当前位姿相减得到位姿误差，作为pid调节器的输入量。

2.   pid调节器根据已记录的误差和当前误差分别计算比例、积分和微分调节量，相加后得到调节修正值。如有必要，可以设置阈值进行分段调节。

3.   根据上一帧数据和当前数据计算出基准速度，然后加上速度修正值得到最终速度并返回。  

### 7.3 调参方法

1.   先调节比例系数kp，大约给30左右，云台越轻给越小，若跟随慢则适当增大kp
2.   除非云台调平问题严重，否则一般ki给0即可
3.   kd视情况调节，若震荡大则适当增大kd
4.   加入卡尔曼预测后需重新微调pid参数
5.   一般pitch轴以位置环控制，所以主要调节yaw轴参数即可



## 8. Rune  能量机关自瞄库

###8.1  功能介绍

该能量机关自瞄库使用传统视觉方式对输入的图像进行处理，通过对二值图的轮廓特征筛选，根据待激活叶片在过去一段时间内的时间戳和角度数据进行函数拟合和预测，并计算预测位姿。在状态机中可直接调用完整流程接口获取最终的控制位姿和自动发射许可。

### 8.2   模块说明

####8.2.1  RuneSingle

RuneSingle 为能量识别类，主要采用了传统识别方法。

#### 8.2.2  API

主接口：

```c++
/**
 * @brief 大符识别主接口，单目
 * @param frame 图像
 * @param vane 能量机关数据
 * @return 识别是否成功
 */
bool detectVane(MatWithTime &frame, Vane2f &vane);
```

其中 `frame` 为单目图像，若图像输入为双目，则需要声明两个该类变量分别进行识别；`vane` 为最终输出相机平面的能量机关数据，通过地址传递，主要包括时间戳、叶片坐标、中心点坐标和单目测距角点。

其他接口如下:

```c++
/**
 * @return 获取已激活的叶片数量
 */
int getNumActivated();
```

返回当前已激活的叶片数量，可供自动击发部分作逻辑判断。

### 8.3 类定义

##### Vane2f

二维能量机关数据类

```c++
class Vane2f
{
public:
    Vane2f();
    Vane2f(cv::Point2f point, float big_contour_rect_ratio, cv::RotatedRect big_rect, cv::RotatedRect small_rect, cv::Point2f base_point);

    void print(std::string tag);

    VaneLightType m_type;           // 叶片类型
    double m_angle;                 // 叶片角度，范围[-PI, Pi]，以 x 轴正方向为零点，逆时针为正向
    double m_time;                  // 叶片被拍摄的时间，单位秒
    cv::Point2f m_point2f;          // 目标坐标
    cv::Point2f m_center2f;         // 中心点坐标

    cv::RotatedRect m_big_rect;             // 叶片矩形，坐标在ROI图上
    cv::RotatedRect m_small_rect;           // 装甲板矩形，坐标在ROI图上
    std::vector<cv::Point2f> m_vertices;    // 单目测距所用灯条角点
};
```

### 8.4 原理

识别的主要过程可以用以下的流程图表示

![流程图](/home/peter/picture/RuneSingle.png)



###8.5 RuneResolver

RuneResolver 为能量机关解算类，采用的核心算法为多元线性回归模型。

#### 8.5.1 API

```c++
/**
 * @brief 能量机关旋转解算主接口
 * @param target_vane 识别到的叶片
 * @param cur_pose 当前位姿
 * @param predict_pose 预测位姿
 * @param state 能量机关旋转模式
 * @param bullet_speed 当前射速
 * @return 解算是否成功
 */
bool resolveRune(Vane3f &target_vane, GimbalPose &cur_pose, GimbalPose &predict_pose, ROBO_ENERGY state, double bullet_speed);
```

其中 `target_vane` 为通过测距计算的相机系三维大符数据，主要包括时间戳、叶片坐标、中心点坐标和角度。`cur_pose`, `state` 和 `bullet_speed` 均通过通讯接口直接获取，`predict_pose` 即为最终输出的控制位姿，通过地址传递。

其他接口如下:

```c++
/**
 * @brief 清空数据集
 */
void clearDataset();

/**
 * @return 预测子弹飞行到击中所需要的时间，单位为秒
 */
double hitTimeCost();
```



##9.双目测距

### 9.1 测距流程

1. 根据双目相机标定的结果，给测距类赋值
2. 对传入的像素坐标点进行矫正，并转换到图像坐标系
3. 求出视差，求出目标点的深度信息
4. 根据焦距深度信息等求出目标点的三维坐标
5. 测量装甲板角度

### 9.2  所需参数文件

Binocular.yaml ———— 双目标定文件



## 10. libBase仓库

### 10.1 宏

- 用于调试 `HINT`  (在任意行添加HINT会打印HINT所在文件以及行数，形成类似于断点的功能)
- 数学 `PI`

### 10.2  类

- 帧率控制 `Rate`
- 带时间戳图像帧 `MatWithTime`
- 云台位姿角 `GimbalPose`

### 10.3  函数

- 获取当前时间戳 `now()`
- 获取当前时间戳（微秒为单位） `getTimeUSecNow()`
- 终端按键监听 `monitorKeyboard()`
- 彩色化终端输出 `_colors()`



## 11. 状态机

###11.1 状态机初始化各对象及参数

```c++
m_robot_control    = std::make_shared<wmj::WMJRobotControl>();
m_usb_capture      = std::make_shared<wmj::UsbCaptureSystem>(USBCAPTURE_CFG);
m_angle_solver     = std::make_shared<wmj::AngleSolver>();
m_speed_resolver   = std::make_shared<wmj::SpeedResolver>();
m_motion_predictor = std::make_shared<wmj::MotionPredict>();
m_armor_detector   = std::make_shared<wmj::ArmorDetectorDouble>();
m_rune_detector    = std::make_shared<wmj::ArmorTrigger>();
m_top_aimer        = std::make_shared<wmj::AngleAimer>();
m_usb_capture->cameraMode("armor");
m_recorders.resize(m_usb_capture->m_device_number);
```

###11.2状态机循环

![图片](/home/peter/picture/state_machine.png)

### 11.3 各项状态流程

#### 11.3.1 自瞄流程

![](/home/peter/picture/state_armor.png)

#### 11.3.2**自动曝光流程**

![图片](![img](/home/peter/picture/autoexposure.png)



## 12.WMJ 通讯模块

### 12.1 模块结构

Port 为通讯基类，SerialPort 和 CanPort 为派生类。WMJProtocol.h 为通讯协议总纲。

### 12.2 主要调用

```cpp
typedef std::vector<uint8_t> Buffer;
```

####12.2.1  读数据函数

```c++
bool Port::sendFrame(Buffer &buffer)
```

参数：

- buffer: 需要发送的数据包

返回：

是否成功读取数据

####12.2.2  写数据函数

```c++
Buffer Port::readFrame(int)
```

参数：

- int: 读取数据来源的设备名

返回：

数据包

### 12.3  核心原理

![](/home/peter/picture/transport.png)



### 12.4 调试方法

- 先判断是否成功读写数据，再检查数据格式和数据内容是否吻合
- 可以适当加入 HINT
- 如果是串口模式可以直接进入 cutecom 查看原始数据。
- 一般需要电控协助



###12.5 参数

- Port.yaml
  - SerialPort
    - 通过 cutecom 查验读写地址
    - 波特率一般默认460800。注意如果是首次打开 cutecom 需要手动改设置，后续会保存设置。
  - CanPort
    - 设备名默认 can0
- Control.yaml
  - port_type 根据电控所安装设备选择通讯模式























