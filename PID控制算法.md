# PID控制算法

### 1.基本概念

PID(proportion integration differentiation)其实就是指比例，积分，微分控制。总的来说，当得到系统的输出后，将输出经过比例，积分，微分3种运算方式，叠加到输入中，从而控制系统的行为。

![PID控制算法原理](https://img-blog.csdn.net/20180711210553477?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1MzUyOTgx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

公式：

![PID控制算法公式](https://img-blog.csdn.net/20180711210623129?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1MzUyOTgx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 2.比例控制算法

给当前误差乘以一个比例系数加到当前值中，达到使当前值不断接近理想值的目的。由于系统存在惯性，系数过大会导致系统振荡加强，而系数过小会导致响应速度减慢。但存在静差无法消除。

### 3.积分控制算法

目的在于消除静差，注意惯性大的物体积分作用应该弱，即T1大。积分进一步缩小了误差，但使得系统稳定裕度减小。

### 4.微分控制算法

微分能反映变化趋势，能在由于目标值变化导致误差变化太大之前，在系统中引入一个有效的早期修正变量提前控制，一般起辅助作用。

### 5.简化公式

![这里写图片描述](https://img-blog.csdn.net/2018071210460749?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1MzUyOTgx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 6.动图演示

![img](https://img-blog.csdnimg.cn/20181216103950234.gif)

### 7.代码展示

#### 7.1结构体初始化

```c++
typedef struct
{
    float target_val;               //目标值
    float actual_val;        		//实际值
    float err;             			//定义偏差值
    float err_last;          		//定义上一个偏差值
    float Kp,Ki,Kd;          		//定义比例、积分、微分系数
    float integral;          		//定义积分值
}pid;

```

#### 7.2位置式PID代码实现



![在这里插入图片描述](https://img-blog.csdnimg.cn/2e0b104a7e7c4d92af972e0e5f707932.png#pic_center)

```c++
float PID_realize(float temp_val)
{
	/*计算目标值与实际值的误差*/
    pid.err=pid.target_val-temp_val;
	/*误差累积*/
    pid.integral+=pid.err;
	/*PID算法实现*/
    pid.actual_val=pid.Kp*pid.err+pid.Ki*pid.integral+pid.Kd*(pid.err-pid.err_last);
	/*误差传递*/
    pid.err_last=pid.err;
	/*返回当前实际值*/
    return pid.actual_val;
}

```

#### 7.3增量式PID代码实现

![img](https://img-blog.csdnimg.cn/6f6be5c81e0d40ff8584ba1c03463f5c.png#pic_center)

```c++
float PID_realize(float temp_val) 
{
	/*传入目标值*/
	pid.target_val = temp_val;
	/*计算目标值与实际值的误差*/
    pid.err=pid.target_val-pid.actual_val;
	/*PID算法实现*/
	float increment_val = pid.Kp*(pid.err - pid.err_next) + pid.Ki*pid.err + pid.Kd*(pid.err - 2 * pid.err_next + pid.err_last);
	/*累加*/
	pid.actual_val += increment_val;
	/*传递误差*/
	pid.err_last = pid.err_next;
	pid.err_next = pid.err;
	/*返回当前实际值*/
	return pid.actual_val;
}

```

#### 7.4对比区别

​        增量式算法不需要对积分项累加，控制量增量只与近几次的误差有关，计算误差对控制量计算的影响较小。而 位置式算法要对近几次的偏差的进行积分累加，容易产生较大的累加误差；
​        增量式算法得出的是控制量的增量，例如在阀门控制中，只输出阀门开度的变化部分，误动作影响小，必要时还可通过逻辑判断限制或禁止本次输出，不会严重影响系统的工作；而位置式的输出直接对应对象的输出，因此对系统影响较大；

​       增量式算法控制输出的是控制量增量，并无积分作用，因此该方法适用于执行机构带积分部件的对象，如步进电机等，而位置式算法适用于执行机构不带积分部件的对象，如电液伺服阀；
​       在进行 PID 控制时，位置式 PID 需要有积分限幅和输出限幅，而增量式 PID 只需输出限幅。

位置式 PID 优缺点：
优点：位置式 PID 是一种非递推式算法，可直接控制执行机构（如平衡小车），u(k) 的值和执行机构的实际位置（如小车当前角度）是一一对应的，因此在执行机构不带积分部件的对象中可以很好应用;
缺点：每次输出均与过去的状态有关，计算时要对 e(k) 进行累加，运算工作量大。

增量式 PID 优缺点：
优点：1. 误动作时影响小，必要时可用逻辑判断的方法去掉出错数据。

2. 手动/自动切换时冲击小，便于实现无扰动切换。
3. 算式中不需要累加。控制增量 Δu(k) 的确定仅与最近 3 次的采样值有关。在速度闭环控制中有很好的实时性。
缺点：1. 积分截断效应大，有稳态误差；
2. 溢出的影响大。有的被控对象用增量式则不太好；

------------------------------------------------
