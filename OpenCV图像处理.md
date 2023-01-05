### 1.腐蚀与膨胀

####1.1 膨胀定义

用结构元素B覆盖的==最大像素值==替换B锚点处的像素，B可以是任意形状。取每个位置领域内最大值，所以膨胀后输出图像的总体亮度的平均值比起原图会有所升高，**图像中比较亮的区域的面积会变大，而较暗物体的尺寸会减小甚至消失**。

#### 1.2API

#####1.2.1结构元B构造函数

getStructuringElement(  int shape    ,   Size ksize    ,    Point anchor);

**返回指定形状和尺寸的结构元素（内核矩阵）**

参数

shape    1）MORPH_RECT    矩形

​               2）MORPH_ELLIPSEM    椭圆形

​               3）MORPH_CROSS   十字交叉形

ksize:       结构体的大小，必须是奇数

anchor：表示结构元的锚点，即参考点。默认值Point(-1, -1)代表中心像素为锚点

##### 1.2.2膨胀函数

dilate(   )

参数

src：表示输入矩阵

element：表示结构元，即 函数getStructuringElement( )的返回值

anchor：结构元的锚点，即参考点

iterations：膨胀操作的次数，默认为一次

borderType：边界扩充类型

borderValue：边界扩充值

#####1.2.3参考代码

```c++
#include <opencv2/opencv.hpp> 
#include <iostream> 
using namespace cv;
 
Mat src, dst;                   // 全局变量
int element_size = 3;      //全局变量
int max_size = 21;           // 全局变量
 
void CallBack_func(int, void*);
int main( ) 
{
	src = imread("test7.png");
	if (src.empty()) 
	{
		printf("could not load the  image...\n");
		return -1;
	}
	namedWindow("原图：", CV_WINDOW_AUTOSIZE);
	imshow("原图：", src);
	namedWindow("膨胀操作后：", CV_WINDOW_AUTOSIZE);
	createTrackbar("结构元尺寸 ：", "膨胀操作后：", &element_size, max_size, CallBack_func);        
	CallBack_func(element_size, 0);
 
	waitKey(0);
	return 0;
}
 
 
void CallBack_func(int, void*) 
{
	int s = element_size * 2 + 1;
	Mat structureElement = getStructuringElement(MORPH_RECT, Size(s, s), Point(-1, -1));      //创建结构元
	dilate(src, dst, structureElement, Point(-1, -1), 1);               //调用膨胀API
	imshow("膨胀操作后：", dst);
}
```

#### 1.3腐蚀定义

 腐蚀操作与膨胀操作类似，只是它取结构元所指定的领域内值的==最小值==作为该位置的输出灰度值。因为取每个位置领域内最小值，所以腐蚀后输出图像的总体亮度的平均值比起原图会有所降低，图像中**比较亮的区域的面积会变小甚至消失，而较暗物体的尺寸会扩大**。

#### 1.4API

erode()

参数

src：表示输入矩阵

dst：表示输出矩阵

element：表示结构元，即 函数getStructuringElement( )的返回值

anchor：结构元的锚点，即参考点

iterations：腐蚀操作的次数，默认为一次

borderType：边界扩充类型

borderValue：边界扩充值

##### 1.4.1参考代码

```c++
#include <opencv2/opencv.hpp> 
#include <iostream> 
using namespace cv;
 
Mat src, dst;                   // 全局变量
int element_size = 3;      //全局变量
int max_size = 21;           // 全局变量
 
void CallBack_func(int, void*);
int main( ) 
{
	src = imread("test7.png");
	if (src.empty()) 
	{
		printf("could not load the  image...\n");
		return -1;
	}
	namedWindow("原图：", CV_WINDOW_AUTOSIZE);
	imshow("原图：", src);
	namedWindow("膨胀操作后：", CV_WINDOW_AUTOSIZE);
	createTrackbar("结构元尺寸 ：", "膨胀操作后：", &element_size, max_size, CallBack_func);        
	CallBack_func(element_size, 0);
 
	waitKey(0);
	return 0;
}
 
 
void CallBack_func(int, void*) 
{
	int s = element_size * 2 + 1;
	Mat structureElement = getStructuringElement(MORPH_RECT, Size(s, s), Point(-1, -1));      //创建结构元
	erode(src, dst, structureElement);               //调用腐蚀API
	imshow("膨胀操作后：", dst);
}
```
### 2.形态学操作

#### 2.1定义

利用基本的膨胀和腐蚀技术，来实现更加高级的形态学变换，如开闭运算、形态学梯度、“顶帽”和“黑帽”等。

#### 2.2API

morphologyEX( src,  dst,   int  op   ,  kernel  )

参数

第一个参数，InputArray类型的src，输入图像，即源图像，填Mat类的对象即可。图像位深应该为以下五种之一：CV_8U, CV_16U,CV_16S, CV_32F 或CV_64F。

第二个参数，OutputArray类型的dst，即目标图像，函数的输出参数，需要和源图片有一样的尺寸和类型。

第三个参数，int类型的op，表示形态学运算的类型，可以是如下之一的标识符：

​              MORPH_OPEN – 开运算（Opening operation）

​              MORPH_CLOSE – 闭运算（Closing operation）

​              MORPH_GRADIENT -形态学梯度（Morphological gradient）

​              MORPH_TOPHAT - “顶帽”（“Top hat”）

​              MORPH_BLACKHAT - “黑帽”（“Black hat")

  第四个参数，InputArray类型的kernel，形态学运算的内核。**若为NULL时，表示的是使用参考点位于中心3x3的核**。我们一般使用函数 ==getStructuringElement==配合这个参数的使用。getStructuringElement函数会返回指定形状和尺寸的结构元素（内核矩阵）。

####2.3效果

开操作：内核矩阵面积增大到一定程度后，白色小斑点消失。

闭操作：内核矩阵面积增大到一定程度后，黑色小斑点消失。

形态学梯度：用膨胀结果减去腐蚀结果，得到图像物体的边界，内核矩阵面积越大，边界越粗。

顶帽：原图像减去开运算结果，内核矩阵面积增大到一定程度后，只留下白色斑点。

黑帽：原图像减去闭运算结果，内核矩阵面积增大到一定程度后，只留下黑色斑点。

####2.4参考代码

```c++
//黑帽变换

#include <opencv2/opencv.hpp>
#include <iostream>
#include <math.h>
 
using namespace cv;
using namespace std;
 
Mat src, dst,dst2;       //定义输入图像和输出图像（三个全局变量）
Mat kernel;         // 定义结构元
int r=1;             //定义结构元半径
int r_max=20;		   //定义结构元最大半径
string window = "闭运算";
 
void callBack_func(int, void*);
 
int main( )
{
	src = imread("test10.png");
	if (src.empty())
	{
		printf("could not load image...\n");
		return -1;
	}
	namedWindow("原图：", CV_WINDOW_AUTOSIZE);
	imshow("原图：", src);
	namedWindow(window, CV_WINDOW_AUTOSIZE);
	createTrackbar("半径", window, &r,r_max, callBack_func);
	callBack_func(0,0);
	waitKey(0);
	return 0;
}
 
void callBack_func(int ,void *)
{
	Mat kernel = getStructuringElement(MORPH_RECT, Size(2*r+1, 2*r+1));     //创建结构元
	morphologyEx(src, dst, MORPH_CLOSE, kernel, Point(-1, -1), 1);           // 形态学处理--闭运算
	imshow(window,dst);                  // 显示形态学处理后的效果
	morphologyEx(src, dst2, MORPH_BLACKHAT, kernel, Point(-1, -1), 1);       // 形态学处理--底帽变换
	imshow("底帽", dst2);               // 显示底帽变换后的效果
}
```

### 3.图像阈值(  threshold  )

#### 3.1基本概念

阈值化就是将图像的所有像素点按照规定的方式赋值成0或者255，使之成为仍可以反映图像整体和局部特征的二值化图像。

#### 3.2API

全局阈值化：

Threshold ( src  ,   dst   ,int threshould_value  ,int threshould_max   ,   type  )

src    源图像，可以为8位的灰度图，也可以为32位的彩色图像。（两者由区别）

dst      输出图像 

threshould_value      阈值

threshould_max      赋给图像中的最大值 

type    阈值类型  （共八种）

THRESH_BINARY      THRESH_BINARY_INV 	    THRESH_TRUNC 	  THRESH_TOZERO 	   THRESH_TOZERO_INV	    THRESH_MASK  	THRESH_OTSU  	       THRESH_TRIANGLE）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301165608409.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301165617511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU4ODE3MQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301165630740.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU4ODE3MQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200301165636535.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU4ODE3MQ==,size_16,color_FFFFFF,t_70)

局部阈值化

adaptiveThreshold(   )

第一个参数，src        输入图像，需为8位单通道浮点图像；

第二个参数，dst        输出图像，需和原图像尺寸类型一致；

第三个参数，max_value             double类型，给像素赋的满足条件的非零值；

第四个参数，adaptive_method        用于指定自适应阈值的算法，CV_ADAPTIVE_THRESH_MEAN_C 或者CV_ADAPTIVE_THRESH_GAUSSIAN_C

 第五个参数，shreshould          取阈值类型：必须是CV_THRESH_BINARY或者CV_THRESH_BINARY_INV

第六个参数，block_size      用来计算阈值的象素邻域大小: 3, 5, 7, ...

第七个参数，param            与方法有关的参数。对方法 CV_ADAPTIVE_THRESH_MEAN_C 和 CV_ADAPTIVE_THRESH_GAUSSIAN_C， 它是一个从均值或加权均值提取的常数, 有时也可以是小数或负数。

注：对方法 CV_ADAPTIVE_THRESH_MEAN_C，先求出块中的均值，再减掉param。
 对方法 CV_ADAPTIVE_THRESH_GAUSSIAN_C ，那么区域中（x，y）周围的像素根据高斯函数按照他们离中心点的距离进行加权计算， 再减掉param。

####3.3参考代码

```c++
#include "stdafx.h"
#include<opencv2/opencv.hpp>
#include<iostream>
#include<math.h>
 
using namespace cv;
Mat src, dst, dst1;
int threshold_value = 127;
int threshold_max = 255;
int type = 2;
int type_max = 4;
char result[] = "threshold image";//或者定义一个指针形式：const char* result="threshold image";
void threshold_Demo(int, void*);
int main(int argc, int argv)
{
	src = imread("C:\\Users\\59235\\Desktop\\imag\\girl1.jpg");
	if (!src.data)
	{
		printf("could not load image...\n");
		return -1;
	}
	namedWindow("input", CV_WINDOW_AUTOSIZE);
	imshow("input", src);
	namedWindow(result, CV_WINDOW_AUTOSIZE);
	createTrackbar("threshold value", result, &threshold_value, threshold_max, threshold_Demo);
	createTrackbar("threshold type", result, &type, type_max, threshold_Demo);
	threshold_Demo(0, 0);
 
	waitKey(0);
	return 0;
}
//阈值化实现函数
void threshold_Demo(int, void*)
{
	cvtColor(src, dst, CV_BGR2GRAY);
	imshow("GRAY", dst);
    //threshold(dst,dst1,threshold_value,threshold_max,type);
	//阈值化（系统帮忙取，大津法OTSU：THRESH_OTSU;三角形阈值法：THRESH_TRIANGLE）
	threshold(dst, dst1, 0, 255, THRESH_OTSU | type);
	imshow(result, dst1);
}

```

### 4.createTrackbar

#### 4.1概念

在显示图像的窗口中快速创建一个滑动控件，用于手动调节阈值，具有非常直观的效果。

### 4.2API

createTrackbar(   )

参数

形式参数一、trackbarname     滑动棒的名称；

形式参数二、winname       滑动帮依附的图像窗口名称；

形式参数三、value：        滑块初始位置；**取地址**

形式参数四、count：        滑块达到最大位置的值；

形式参数五、TrackbarCallback        回调函数，其定义如下：

```c++
typedef void (CV_CDECL *TrackbarCallback)(int pos, void* userdata);
```

#### 4.3参考代码

```c++
#include <opencv2/opencv.hpp>
#include <iostream>
 
using namespace cv;
using namespace std;
 
int i = 7;//滑动条初始值
int maxnum = 20;//滑动条最大值
void text(int,void*);//声明回调函数
 
int main(int argc, char** argv) 
	{
	Mat src = imread("image5.jpg");
//判断图片是否载入成功
	if (src.empty()) 
	{
		printf("图片加载失败\n");
		system("pause");
		//return -1;
	}
	//新建一个窗口
	namedWindow("测试窗口",WINDOW_AUTOSIZE);
	//创建滑动条
	//注意：i是变量，滑动条擦改变后i改变。
	createTrackbar("数字：","测试窗口",&i,maxnum,text);
 
    text(0,0);
 
	waitKey(0);
	return 0;
}
//回调函数
void text(int,void*)
{
	cout<<"数字i的值为:"<<i<<endl;
}
```

### 5.Harris角点特征

#### 5.1角点定义

角点的定义一般分为以下三种：图像边界曲线上具有极大曲率值的点；图像中梯度值和梯度变化率都很高的点；图像边界方向变化不连续的点。

#### 5.2角点的检测方法

角点检测方法主要有２大类：

 １）基于图像边缘轮廓特征的方法。此方法需要经过图像预分割、轮廓链码提取和角点检测，如基于边界曲率的角点检测，基于边界小波变换的角点检测以及基于边界链码的角点检测。

 ２）基于图像灰度信息的方法。此方法主要通过计算曲率及梯度进行角点检测，通过计算边缘的曲率来判断角点的存在性。典型代表有Harris算法、Susan算法、Moravec算法等。

#### 5.3Harris算法步骤

（1）利用差分算子对图像进行滤波，并计算图像中每个像素点的 ![img](https://img-blog.csdn.net/2018041618143124?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTY5NTU2NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)、![img](https://img-blog.csdn.net/20180416181448257?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTY5NTU2NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)以及![img](https://img-blog.csdn.net/20180416181513552?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTY5NTU2NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)；

（2） 对 ![img](https://img-blog.csdn.net/2018041618143124?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTY5NTU2NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)、![img](https://img-blog.csdn.net/20180416181448257?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTY5NTU2NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)以及![img](https://img-blog.csdn.net/20180416181513552?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTY5NTU2NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)进行高斯平滑，以去除噪声。在此过程中可通过归一化将模板参数和置 1；

（3） 计算原图像每个对应点的兴趣值 ![img](https://img-blog.csdn.net/20180416181708444?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTY5NTU2NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)，并进行局部非极大值抑制；

（4） 设定阈值，如果兴趣值大于这个阈值并且在某领域内是局部最大值，则认为该点是需要提取的角点。

公式：文中采用如下角点响应函数计算兴趣值从而判定出角点，即：

![img](https://img-blog.csdn.net/20180416181143308?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTY5NTU2NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

   其中，ε 表示任意小的正数。显然采用这中方式计算可以避免 ![img](https://img-blog.csdn.net/20180416180914414?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTY5NTU2NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)选择的随机性。

#### 5.4参考代码

```c++
#include <opencv2/opencv.hpp>
#include <iostream>
#include <math.h>
 
using namespace cv;
using namespace std;
 
/************************************************/
void ConvertBGR_img2GRAY_img(const Mat &BGR_img, Mat &Gray_img)
{
	//这个函数实现将彩色图转化成灰度图,利用常用公式：Gray = R*0.299 + G*0.587 + B*0.114进行自编程实现
	//第一个参数BGR_img表示输入的彩色RGB图像的引用；    
	//第二个参数Gray_img表示转换后输出的灰度图像的引用；                  
	if (BGR_img.empty() || BGR_img.channels() != 3)
	{
		return;
	}
	Gray_img = Mat::zeros(BGR_img.size(), CV_8UC1);       //创建一张与输入图像同大小的单通道灰度图像  
	uchar *pointImage = BGR_img.data;                        //取出存储图像像素的数组的指针  
	uchar *pointImageGray = Gray_img.data;
	int height = BGR_img.rows;
	int width = BGR_img.cols;
	for (int i = 0; i < height; i++)
	{
		for (int j = 0; j < width; j++)
		{
			*(pointImageGray + i*Gray_img.step[0] + j*Gray_img.step[1]) = (uchar)(0.114*(pointImage + i*BGR_img.step[0] + j*BGR_img.step[1])[0] + 0.587*(pointImage + i*BGR_img.step[0] + j*BGR_img.step[1])[1] + 0.299*(pointImage + i*BGR_img.step[0] + j*BGR_img.step[1])[2]);
		}
	}
 
}
/**********************************************/
 
/*******************************************************/
//非极大值抑制和阈值检测
void LocalMaxValue(Mat &resultData, Mat &ResultImage, int kSize)
{
	int r = kSize / 2;
	for (int i = r; i < ResultImage.rows - r; i++)
	{
		for (int j = r; j < ResultImage.cols - r; j++)
		{
			if (resultData.at<double>(i, j) > resultData.at<double>(i - 1, j - 1) &&
				resultData.at<double>(i, j) > resultData.at<double>(i - 1, j) &&
				resultData.at<double>(i, j) > resultData.at<double>(i - 1, j + 1) &&
				resultData.at<double>(i, j) > resultData.at<double>(i, j - 1) &&
				resultData.at<double>(i, j) > resultData.at<double>(i, j + 1) &&
				resultData.at<double>(i, j) > resultData.at<double>(i + 1, j - 1) &&
				resultData.at<double>(i, j) > resultData.at<double>(i + 1, j) &&
				resultData.at<double>(i, j) > resultData.at<double>(i + 1, j + 1))
			{
				if ((int)resultData.at<double>(i, j) >15000)
				{
					circle(ResultImage, Point(j, i), 5, Scalar(0, 0, 255), 2, 8, 0);
				}
			}
		}
	}
}
/*********************************************************/
 
//  主函数
int main()
{
	const Mat src_img = imread("test11.png");
	if (src_img.empty())
	{
		printf("could not load image...\n");
		return -1;
	}
	namedWindow("原图：", CV_WINDOW_AUTOSIZE);
	imshow("原图：", src_img);
 
	// 将彩色图转化为灰度图，调用OpenCV提供的cvtColor接口
	Mat gray_img;
	ConvertBGR_img2GRAY_img(src_img, gray_img);
	namedWindow("灰度图", CV_WINDOW_AUTOSIZE);
	imshow("灰度图", gray_img);
 
	// 第一步：用sobel算子计算度图像的水平和垂直方向上的差分
	Mat image_con_sobel_Ix, image_con_sobel_Iy;
	//image_con_sobel_Ix = sobel(gray_img,1,0, 3, BORDER_DEFAULT);
	//image_con_sobel_Iy = sobel(gray_img,0,1, 3, BORDER_DEFAULT);
	Sobel(gray_img, image_con_sobel_Ix, CV_64F, 1, 0, 3);
	Sobel(gray_img, image_con_sobel_Iy, CV_64F, 0, 1, 3);
 
	//convertScaleAbs(image_con_sobel_Ix, image_con_sobel_Ix);
	//convertScaleAbs(image_con_sobel_Iy, image_con_sobel_Iy);
 
	//  第二步：计算Ixx,Iyy,Ixy
	Mat image_con_sobel_Ixx, image_con_sobel_Iyy, image_con_sobel_Ixy;
	multiply(image_con_sobel_Ix, image_con_sobel_Ix, image_con_sobel_Ixx, 1.0, CV_64FC1);
	multiply(image_con_sobel_Iy, image_con_sobel_Iy, image_con_sobel_Iyy, 1.0, CV_64FC1);
	multiply(image_con_sobel_Ix, image_con_sobel_Iy, image_con_sobel_Ixy, 1.0, CV_64FC1);
 
	//第三步：对Ixx,Iyy,Ixy进行高斯平滑
	Mat blurimage_Ixx, blurimage_Iyy, blurimage_Ixy;
	GaussianBlur(image_con_sobel_Ixx, blurimage_Ixx, Size(3, 3), 5);
	GaussianBlur(image_con_sobel_Iyy, blurimage_Iyy, Size(3, 3), 5);
	GaussianBlur(image_con_sobel_Ixy, blurimage_Ixy, Size(3, 3), 5);
 
	//第四步：计算响应函数,为避免K值的随机性，我采用另外一种响应函数表达式
	Mat Response_data;
	Response_data = (blurimage_Ixx.mul(blurimage_Iyy) - blurimage_Ixy.mul(blurimage_Ixy)) / (blurimage_Ixx + blurimage_Iyy);
 
	//第五步：非极大值抑制和阀值检测    
	Mat ResultImage = gray_img.clone();
	LocalMaxValue(Response_data, ResultImage, 3);
	namedWindow("结果", CV_WINDOW_AUTOSIZE);
	imshow("结果", ResultImage);
	waitKey(0);
	return 0;
}
```

#### 5.5API

cornerHarris(   )

参数

第一个参数，InputArray类型的src，输入图像，即源图像，填Mat类的对象即可，且需为单通道8位或者浮点型图像。

第二个参数，OutputArray类型的dst，函数调用后的运算结果存在这里，即这个参数用于存放Harris角点检测的输出结果，和源图片有一样的尺寸和类型。

第三个参数，int类型的blockSize，表示邻域的大小，对每个像素，考虑blockSize×blockSize大小的邻域，在邻域上计算图像的2x2梯度的协方差矩阵M。

第四个参数，int类型的ksize，为Soble算子核尺寸，如果小于0，采用3×3的Scharr滤波器

第五个参数，double类型的k，为角点响应函数中的经验常数（0.04~0.06）。

第六个参数，int类型的borderType，图像像素的边界模式，注意它有默认值BORDER_DEFAULT。更详细的解释，参考borderInterpolate( )函数。

```c++
#include <opencv2/opencv.hpp>
#include <iostream>
#include <math.h>
 
using namespace cv;
using namespace std;
 
 
 
 
string out_title = "Harris角点检测";
int thresh = 100;
int max_count = 255;
Mat src_img, gray_img;
void Harris_callback(int, void*);
 
int main()
{
	src_img = imread("test11.png");
	if (src_img.empty())
	{
		printf("could not load image...\n");
		return -1;
	}
	namedWindow("原图：", CV_WINDOW_AUTOSIZE);
	imshow("原图：", src_img);
	namedWindow(out_title, CV_WINDOW_AUTOSIZE);
	cvtColor(src_img, gray_img, CV_BGR2GRAY);
	createTrackbar("阈值", out_title, &thresh, max_count, Harris_callback);
	Harris_callback(0, 0);
	waitKey(0);
	return 0;
}
 
void Harris_callback(int, void*)
{
	Mat dst_img, norm_dst_img, scale_dst_img;
	dst_img = Mat::zeros(gray_img.size(), CV_32FC1);
	int  blockSize = 2;
	int ksize = 3;
	double k = 0.04;
	cornerHarris(gray_img, dst_img, blockSize, ksize, k, BORDER_DEFAULT);   //调用API
	normalize(dst_img, norm_dst_img, 0, 255, NORM_MINMAX, CV_32FC1, Mat());     // 调用归一化函数进行归一化处理
	convertScaleAbs(norm_dst_img, scale_dst_img);      //将图像转化为8位图
	Mat result_img = src_img.clone();
	for (int r = 0; r < result_img.rows; ++r)
	{
		uchar *ptr_row = scale_dst_img.ptr<uchar>(r);
		for (int c = 0; c < result_img.cols; ++c)
		{
			int value = int(ptr_row[c]);
			if (value > thresh)
			{
				circle(result_img, Point(c, r), 2, Scalar(0, 0, 255), 2, 8, 0);
			}
		}
	}
	imshow(out_title, result_img);
}
```

### 6.卷积操作

#### 6.1边界补充问题

####6.1.1API

copyMakeBorder(   )

参数

src：要处理的原图    dst   要得到的图像

 top, bottom, left, right：上下左右要扩展的像素数
 borderType：边框类型。包括

对称填充        BORDER_REFLECT

边界复制填充   BORDER_REPLICATE

镜像填充（也是默认填充方式）BORDER_REFLECT_101

 固定值填充   BORDER_CONSTANT  （后面要跟给定值）



#### 6.2平滑均值滤波

![这里写图片描述](https://img-blog.csdn.net/20180411211352781?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYWlwcDA2MDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 6.3高斯平滑

![这里写图片描述](https://img-blog.csdn.net/20180701184737562?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYWlwcDA2MDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 6.4图像锐化

![这里写图片描述](https://img-blog.csdn.net/20180411211719651?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYWlwcDA2MDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![这里写图片描述](https://img-blog.csdn.net/20180411211740826?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYWlwcDA2MDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 6.5梯度Prewitt

梯度Prewitt卷积核与Soble卷积核的选定是类似的，都是对水平边缘或垂直边缘有比较好的检测效果。

#####6.5.1水平梯度

![这里写图片描述](https://img-blog.csdn.net/2018041121180512?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYWlwcDA2MDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### 6.5.2垂直梯度

![这里写图片描述](https://img-blog.csdn.net/201804112120112?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYWlwcDA2MDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 6.6Sobel边缘检测

Soble与上述卷积核不同之处在于，Soble更强调了和边缘相邻的像素点对边缘的影响。

##### 6.6.1水平梯度

![这里写图片描述](https://img-blog.csdn.net/2018041121220776?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYWlwcDA2MDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### 6.6.2垂直梯度

![这里写图片描述](https://img-blog.csdn.net/20180411212247770?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYWlwcDA2MDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 6.7梯度Laplacian

Laplacian也是一种锐化方法，同时也可以做边缘检测，而且边缘检测的应用中并不局限于水平方向或垂直方向，这是Laplacian与soble的区别。

![这里写图片描述](https://img-blog.csdn.net/20180411212348759?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NoYWlwcDA2MDc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 6.8API

filter2D(  )

参数

第一个参数: 输入图像
第二个参数: 输出图像，和输入图像具有相同的尺寸和通道数量
第三个参数: 目标图像深度，输入值为-1时，目标图像和原图像深度保持一致。
第四个参数: 卷积核，是一个矩阵
第五个参数：内核的基准点(anchor)，其默认值为(-1,-1)说明位于kernel的中心位置。基准点即kernel中与进行处理的像素点重合的点。
第五个参数: 在储存目标图像前可选的添加到像素的值，默认值为0
第六个参数: 像素向外逼近的方法，默认值是BORDER_DEFAULT。

#### 6.9参考代码

```c++
#include <iostream>
#include <iostream>
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
using namespace std;
using namespace  cv;

int main()
{
    Mat srcImage = imread("1.jpg");
    namedWindow("srcImage", WINDOW_AUTOSIZE);
    imshow("原图", srcImage);
    Mat kernel = (Mat_<double>(3,3) << 
        -1, 0 ,1,
        -2, 0, 2,
        -1, 0, 1);
    Mat dstImage;
    filter2D(srcImage,dstImage,srcImage.depth(),kernel);
    namedWindow("dstImage",WINDOW_AUTOSIZE);
    imshow("卷积图",dstImage);
    waitKey(0);
    return 0;
}
```



###7.Soble算子

#### 7.1原理

![img](https://img-blog.csdn.net/20160715164950561?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

![img](https://img-blog.csdn.net/20160715165015108?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

#### 7.2作用

Sobel算子根据像素点上下、左右邻点灰度加权差，在边缘处达到极值这一现象检测边缘。对噪声具有平滑作用，提供较为精确的边缘方向信息，边缘定位精度不够高。当对精度要求不是很高时，是一种较为常用的边缘检测方法。

#### 7.3API

Sobel(  )

第一个参数，InputArray 类型的src，为输入图像，填Mat类型即可。

第二个参数，OutputArray类型的dst，即目标图像，函数的输出参数，需要和源图片有一样的尺寸和类型。

第三个参数，int类型的ddepth,取-1即可。输出图像的深度，支持如下src.depth()和ddepth的组合：

- 若src.depth() = CV_8U, 取ddepth =-1/CV_16S/CV_32F/CV_64F

  若src.depth() = CV_16U/CV_16S, 取ddepth =-1/CV_32F/CV_64F

  若src.depth() = CV_32F, 取ddepth =-1/CV_32F/CV_64F

  若src.depth() = CV_64F, 取ddepth = -1/CV_64F

第四个参数，int类型dx，x 方向上的差分阶数。

第五个参数，int类型dy，y方向上的差分阶数。

注：dx与dy和为一，若求x方向梯度，dx取1，dy取0；若求y方向梯度，dy取1，dx取0；若求合并梯度，均取0.5可得近似结果。

第六个参数，int类型ksize，有默认值3，表示Sobel核的大小;必须取1，3，5或7。

第七个参数，double类型的scale，计算导数值时可选的缩放因子，默认值是1，表示默认情况下是没有应用缩放的。我们可以在文档中查阅getDerivKernels的相关介绍，来得到这个参数的更多信息。

第八个参数，double类型的delta，表示在结果存入目标图（第二个参数dst）之前可选的delta值，有默认值0。

第九个参数， int类型的borderType，边界模式，默认值为BORDER_DEFAULT。

```汉语
在卷积开始之前增加边缘像素，填充的像素值为0 或者 RGB黑色，比如在 ( 3 ∗ 3 ) (3 * 3) (3∗3)的四周各填充一个像素的边缘，这样就缺包图像的边缘被处理，在卷积之后再去掉这些边缘。opencv中默认的处理方法是：BorderTypes.Default ，除此之外常用的还有：
BorderTypes.Constant；填充边缘用指定的像素值。
BorderTypes.Replicate；填充边缘像素用已知的边缘像素值。
BorderTypes.Wrap；用另一边的像素来补偿填充。。
```

#### 7.4参考代码

```c++
#include<opencv2/opencv.hpp>
using namespace cv;
int main()
{
	Mat dstIamge, grad_x, grad_y, abs_grad_x, abs_grad_y;
	Mat srcIamge = imread("D:\\vvoo\\lena.jpg");
 
	imshow("原始图", srcIamge);
	cvtColor(srcIamge, srcIamge, COLOR_RGB2GRAY);
	//求 X方向梯度
	Sobel(srcIamge, grad_x, CV_16S, 1, 0, 3, 1, 1, BORDER_DEFAULT);
	convertScaleAbs(grad_x, abs_grad_x);//使用线性变换转换输入数组元素成8位无符号整型
	imshow("X方向Sobel", abs_grad_x);
 
	//求Y方向梯度
	Sobel(srcIamge, grad_y, CV_16S, 0, 1, 3, 1, 1, BORDER_DEFAULT);
	convertScaleAbs(grad_y, abs_grad_y);
	imshow("Y方向Sobel", abs_grad_y);
 
	//合并梯度(近似)
	addWeighted(abs_grad_x, 0.5, abs_grad_y, 0.5, 0,dstIamge);
	imshow("整体方向Sobel", dstIamge);
 
	waitKey(0);
	return 0;
}
```

### 8.寻找图像轮廓

#### 8.1基本概念

当我们通过阈值分割提取到图像中的目标物体后，我们就需要通过边缘检测来提取目标物体的轮廓，使用这两种方法基本能够确定物体的边缘或者前景。接下来，我们通常需要做的是拟合这些边缘的前景，如拟合出包含前景或者边缘像素点的最小外包矩形、圆、凸包等几何形状，为计算它们的面积或者模板匹配等操作打下坚实的基础。

#### 8.2API

##### 8.2.1查找轮廓

findContour(   )

参数

image：单通道图像矩阵，可以是灰度图，但更常用的是==二值图像==，一般是经过Canny、拉普拉斯等边缘检测算子处理过的二值图像；

contours                          vector<vector<Point>>类型，是一个向量，并且是一个==双重向量==，向量内每个元素保存了一组由连续的Point点构成的点的集合的向量，**每一组Point点集就是一个轮廓**。有多少轮廓，向量contours就有多少元素。

hierarchy：vector<Vec4i> 类型， Vec4i是Vec<int,4>的别名，即容器内每一个元素都是一个包含了4个int型变量的向量，所以从定义上看，hierarchy也是一个向量，向量内每个元素保存了一个包含4个int整型的数组。向量hiararchy内的元素和轮廓向量contours内的元素是一一对应的，向量的容量相同。hierarchy向量内每一个元素的4个int型变量——hierarchy【i】【0】 ~hierarchy【i】【3】，**分别表示第i个轮廓的后一个轮廓、前一个轮廓、父轮廓、内嵌轮廓的索引编号**。如果当前轮廓没有对应的后一个轮廓、前一个轮廓、父轮廓或内嵌轮廓的话，则hierarchy【i】【0】 ~hierarchy【i】【3】的相应位被设置为默认值-1。

mode：int类型的，定义轮廓的检索模式：

 取值一：CV_RETR_EXTERNAL只检测最外围轮廓，包含在外围轮廓内的内围轮廓被忽略；

  取值二：CV_RETR_LIST  检测所有的轮廓，包括内围、外围轮廓，但是检测到的轮廓不建立等级关系，彼此之间独立，没有等级关系，这就意味着这个检索模式下不存在父轮廓或内嵌轮廓，所以hierarchy向量内所有元素的第3、第4个分量都会被置为-1。

取值三：CV_RETR_CCOMP  检测所有的轮廓，但所有轮廓只建立两个等级关系，外围为顶层，若外围内的内围轮廓还包含了其他的轮廓信息，则内围内的所有轮廓均归属于顶层；

取值四：CV_RETR_TREE， 检测所有轮廓，所有轮廓建立一个等级树结构。外层轮廓包含内层轮廓，内层轮廓还可以继续包含内嵌轮廓。

（5） method：int类型，定义轮廓的近似方法：

 取值一：CV_CHAIN_APPROX_NONE 保存物体边界上所有连续的轮廓点到contours向量内；

取值二：CV_CHAIN_APPROX_SIMPLE 仅保存轮廓的拐点信息，把所有轮廓拐点处的点保存入contours向量内，拐点与拐点之间直线段上的信息点不予保留；

 取值三和四：CV_CHAIN_APPROX_TC89_L1，CV_CHAIN_APPROX_TC89_KCOS使用teh-Chinl chain 近似算法；

（6） Point：偏移量，所有的轮廓信息相对于原始图像对应点的偏移量，相当于在每一个检测出的轮廓点上加上该偏移量，并且Point还可以是负值。

##### 8.2.2绘制轮廓

drawContours(   )

参数

（1）image: 代表输入的图像矩阵，将轮廓画在该图上；

（2）contours：是得到的一系列点的集合，代表多个轮廓；

（3）contourIdx：是一个索引，代表绘制contours中的第几个轮廓；

（4） color：被填充的颜色，单色可以设置为Scalar(255)等；

（5）thickness: 所画Contour的线条粗细，如果该参数值小于0，则表示填充整个轮廓内的区域；

（6）lineType： 线的连通性；

（7）hierarchy：可选层次信息结构，这里面是findContours所得到的基于Contours的层级信息；

（8）maxLevel： 绘制轮廓的最大等级。如果等级为0，绘制单独的轮廓。如果为1，绘制轮廓及在其后的相同的级别下轮廓。如果值为2，所有的轮廓。如果等级为2，绘制所有同级轮廓及所有低一级轮廓，诸此种种。如果值为负数，函数不绘制同级轮廓，但会升序绘制直到级别为abs(max_level)-1的子轮廓。

（9）offset：照给出的偏移量移动每一个轮廓点坐标.当轮廓是从某些感兴趣区域(ROI)中提取的然后需要在运算中考虑ROI偏移量时，将会用到这个参数。

#### 8.3参考代码

```c++
#include <opencv2/opencv.hpp>
#include <iostream>
#include <math.h>
 
using namespace std;
using namespace cv;
 
Mat src_img, dst_img;
const string output_win = "找到的轮廓";
int threshold_value = 100;
int threshold_max = 255;
RNG rng;
void Demo_Contours(int, void*);
int main() 
{
	src_img = imread("fish.png");
	if (src_img.empty())
	{
		printf("could not load the  image...\n");
		return -1;
	}
	namedWindow("原图", CV_WINDOW_AUTOSIZE);
	namedWindow(output_win, CV_WINDOW_AUTOSIZE);
	imshow("原图", src_img);
	cvtColor(src_img, src_img, CV_BGR2GRAY);
 
	createTrackbar("Threshold Value:", output_win, &threshold_value, threshold_max, Demo_Contours);
	Demo_Contours(0, 0);
 
	waitKey(0);
	return 0;
}
 
void Demo_Contours(int, void*)
{
	Mat canny_output;
	vector<vector<Point>> contours;
	vector<Vec4i> hierachy;
	Canny(src_img, canny_output, threshold_value, threshold_value * 2, 3, false);
	findContours(canny_output, contours, hierachy, RETR_TREE, CHAIN_APPROX_SIMPLE, Point(0, 0));
 
	dst_img = Mat::zeros(src_img.size(), CV_8UC3);
	RNG rng(12345);
	for (auto i = 0; i < contours.size(); ++i)
	{
		Scalar color = Scalar(rng.uniform(0, 255), rng.uniform(0, 255), rng.uniform(0, 255));   //随机颜色
		drawContours(dst_img, contours, i, color, 2, 8, hierachy, 0, Point(0, 0));
	}
	imshow(output_win, dst_img);
}
```

### 9.亚像素角点检测

####9.1基本概念

前面我们介绍了用 Harris提取角点，但是提取的角点是像素级的，精度不高，若我们进行图像处理的目的不是提取用于识别的特征点而是进行几何测量，这通常需要更高的精度。

操作：在 Harris 提取角点过程中，通过两次角点筛选，剔除非角点和伪角点，利用角点响应函数执行非极大值抑制，以局部角点响应函数最大值的像素点作为初始角点，并以该初始角点为中心，以一定半径搜索角点簇，采用最小二乘法加权角点簇与待求角点的欧几里得距离，精化初始角点坐标，从而实现 Harris 亚像素角点准确快速定位。

#### 9.2原理解析

在亚像素级精度的角点检测算法中，有两种常用的方法。一种方法是从亚像素角点到周围像素点的矢量应垂直于图像的灰度梯度这个观察事实得到的，通过最小化误差函数的迭代方法来获得亚像素级精度的坐标值。另一种方法是用二次多项式去逼近周围3× 3领域内的角点反应函数，用线性解法求得亚像素级角点坐标。

#### 9.3API

cornerSubPix(   )

参数

 image：输入图像，即源图像；

corners：提供输入角点的初始坐标和精确的输出坐标。

 winSize：Size类型，表示搜索窗口的半径。若winSize=Size(5，5)，那么就表示使用（5*2+1）x（5*2+1）=11*11大小的搜索窗口。

zeroZone：Size类型，表示死区的一半尺寸。而死区为不对搜索区的中央位置做求和运算的区域，用来避免自相关矩阵出现的某些可能的奇异性。值为（-1，-1）表示没有死区。

 criteria：TermCriteria类型，求角点的迭代过程的终止条件。

#### 9.4参考代码

```c++
#include <opencv2/opencv.hpp>
#include <iostream>
 
using namespace cv;
using namespace std;
 
int max_corners = 20;
int max_count = 50;
Mat src_img, gray_img;
const string output_winName = "亚像素角点检测";
RNG  random_number_generator;             // 定义一个随机数发生器  
void SubPixel_Demo(int, void*);
 
int main() 
{
	src_img = imread("test11.png");
	if (src_img.empty()) 
	{
		printf("could not load the image...\n");
		return -1;
	}
	namedWindow("原图", CV_WINDOW_AUTOSIZE);
	imshow("原图", src_img);
	cvtColor(src_img, gray_img, COLOR_BGR2GRAY);
	namedWindow(output_winName, CV_WINDOW_AUTOSIZE);
	createTrackbar("角点数", output_winName, &max_corners, max_count, SubPixel_Demo);  //创建TrackBar
	SubPixel_Demo(0, 0); 
 
	waitKey(0);
	return 0;
}
 
void SubPixel_Demo(int, void*) 
{
	if (max_corners < 5) 
	{
		max_corners = 5;   //控制下限
	}
	vector<Point2f> corners;           //  corners是一个Vector，里面存放找到的角点坐标，即float的Point值
	double qualityLevel = 0.01;
	double minDistance = 10;
	int blockSize = 3;
	double k = 0.04;
	cout << "************************************************************"<< endl; 
	cout <<                 "作者;行歌"                           << endl;
 
	// 调用goodFeaturesToTrack进行shi-Tomasi角点检测
	goodFeaturesToTrack(gray_img, corners, max_corners, qualityLevel, minDistance, Mat(), blockSize, false, k);
	cout << "角点数: " << corners.size() << endl;
	Mat result_img = src_img.clone();
	for (auto t = 0; t < corners.size();++t)
	{
		circle(result_img, corners[t], 2, Scalar(random_number_generator.uniform(0, 255), 
			random_number_generator.uniform(0, 255), random_number_generator.uniform(0, 255)), 2, 8, 0);    //   注意corners[t]就是一个Point类型的坐标
	}
	imshow(output_winName, result_img);
	// 参数设置
	Size winSize = Size(5, 5);
	Size zerozone = Size(-1, -1);
	TermCriteria criteria = TermCriteria(TermCriteria::EPS + TermCriteria::MAX_ITER, 40, 0.001);
	cornerSubPix(gray_img, corners, winSize, zerozone, criteria);   	//  调用cornerSubPix函数计算出亚像素角点的位置
 
	// 输出亚像素角点信息
	for (auto t = 0; t < corners.size(); ++t)
	{
		cout << "精确角点坐标[" << t +1 << "]" << " point[x, y] = " << corners[t].x<< " , " << corners[t].y  << endl;
	}
}
```

### 10.凸包

#### 10.1基本概念

凸包(Convex Hull)是一个计算几何（图形学）中的概念，在一个实数向量空间V中，对于给定集合，所有包含X的凸集的交集S被称为X的凸包。 在二维欧几里得空间中，凸包可以想象为**一条刚好包着所有点的橡皮圈**，用不严谨的话来讲，给定二维平面上的点集，凸包就是将最外层的点连接起来构成的凸多边形，它能包含点集中所有的点。

#### 10.2API

convexHull(  )

参数

**points:**输入的二维点集，Mat类型数据即可 

**hull:**输出参数，用于输出函数调用后找到的凸包 

**clockwise:**操作方向，当标识符为真时，输出凸包为顺时针方向，否则为逆时针方向。

**returnPoints:**操作标识符，默认值为true，此时返回各凸包的各个点，否则返回凸包各点的指数，当输出数组时std::vector时，此标识被忽略。

#### 10.3参考代码

```c++

    #include <iostream>
    #include <opencv2\core\core.hpp>
    #include <opencv2\highgui\highgui.hpp>
    #include <opencv2\imgproc\imgproc.hpp>

    using namespace std;
    using namespace cv;

    Mat srcImage, grayImage;
    int thresh = 100;
    const int threshMaxValue = 255;
    RNG rng(12345);

    //定义回调函数
    void thresh_callback(int, void*);

    int main()
    {
        srcImage = imread("convexhull.jpg");

        //判断图像是否加载成功
        if (srcImage.empty())
        {
            cout << "图像加载失败" << endl;
            return -1;
        }
        else
        {
            cout << "图像加载成功!" << endl << endl;
        }

        //图像灰度图转化并平滑滤波
        cvtColor(srcImage, grayImage, COLOR_BGR2GRAY);
        blur(grayImage, grayImage, Size(3, 3));

        namedWindow("原图像", WINDOW_AUTOSIZE);
        imshow("原图像", grayImage);

        //创建轨迹条
        createTrackbar("Threshold:", "原图像", &thresh, threshMaxValue, thresh_callback);
        thresh_callback(thresh, 0);
        waitKey(0);

        return 0;
    }

    void thresh_callback(int, void*)
    {
        Mat src_copy = srcImage.clone();
        Mat threshold_output;
        vector<vector<Point>>contours;
        vector<Vec4i>hierarchy;

        //使用Threshold检测图像边缘
        threshold(grayImage, threshold_output, thresh, 255, THRESH_BINARY);

        //寻找图像轮廓
        findContours(threshold_output, contours, hierarchy, RETR_TREE, CHAIN_APPROX_SIMPLE, Point(0, 0));

        //寻找图像凸包
        vector<vector<Point>>hull(contours.size());
        for (int i = 0; i < contours.size(); i++)
        {
            convexHull(Mat(contours[i]), hull[i], false);
        }

        //绘制轮廓和凸包
        Mat drawing = Mat::zeros(threshold_output.size(), CV_8UC3);
        for (int i = 0; i < contours.size(); i++)
        {
            Scalar color = Scalar(rng.uniform(0, 255), rng.uniform(0, 255), rng.uniform(0, 255));
            drawContours(drawing, contours, i, color, 1, 8, vector<Vec4i>(), 0, Point());
            drawContours(drawing, hull, i, color, 1, 8, vector<Vec4i>(), 0, Point());
        }

        namedWindow("凸包", WINDOW_AUTOSIZE);
        imshow("凸包", drawing);
    }

```

### 11.为轮廓创建可倾斜的边界框和椭圆

#### 11.1API

##### 11.1.1矩形

minAreaRect()

作用：找到一个能包围输入二维点集的面积最小的任意方向矩形。

形式：minAreaRect(InputArray points);

参数：points：输入二维点集，并用std::vector or Mat存储；

##### 11.1.2圆形

fitEllipse()

作用：寻找一个适合的围绕二维点集的椭圆。

形式：fitEllipse(InputArray points)；

参数：points：输入二维点集，并用std::vector or Mat存储；



==注意==：两个函数都是返回相应的点集，需要用drawContuors()绘制出来

#### 11.2参考代码

```c++
<span style="font-size:14px;">#include "opencv2/highgui/highgui.hpp"
#include "opencv2/imgproc/imgproc.hpp"
#include <iostream>
#include <stdio.h>
#include <stdlib.h>
 
using namespace cv;
using namespace std;
 
Mat src; Mat src_gray;
int thresh = 100;
int max_thresh = 255;
RNG rng(12345);
 
/// Function header
void thresh_callback(int, void* );
 
/** @function main */
int main( int argc, char** argv )
{
  /// 加载源图像
  src = imread( argv[1], 1 );
 
  /// 转为灰度图并模糊化
  cvtColor( src, src_gray, CV_BGR2GRAY );
  blur( src_gray, src_gray, Size(3,3) );
 
  /// 创建窗体
  char* source_window = "Source";
  namedWindow( source_window, CV_WINDOW_AUTOSIZE );
  imshow( source_window, src );
 
  createTrackbar( " Threshold:", "Source", &thresh, max_thresh, thresh_callback );
  thresh_callback( 0, 0 );
 
  waitKey(0);
  return(0);
}
 
/** @function thresh_callback */
void thresh_callback(int, void* )
{
  Mat threshold_output;
  vector<vector<Point> > contours;
  vector<Vec4i> hierarchy;
 
  /// 阈值化检测边界
  threshold( src_gray, threshold_output, thresh, 255, THRESH_BINARY );
  /// 寻找轮廓
  findContours( threshold_output, contours, hierarchy, CV_RETR_TREE, CV_CHAIN_APPROX_SIMPLE, Point(0, 0) );
 
  /// 对每个找到的轮廓创建可倾斜的边界框和椭圆
  vector<RotatedRect> minRect( contours.size() );
  vector<RotatedRect> minEllipse( contours.size() );
 
  for( int i = 0; i < contours.size(); i++ )
     { minRect[i] = minAreaRect( Mat(contours[i]) );
       if( contours[i].size() > 5 )
         { minEllipse[i] = fitEllipse( Mat(contours[i]) ); }
     }
 
  /// 绘出轮廓及其可倾斜的边界框和边界椭圆
  Mat drawing = Mat::zeros( threshold_output.size(), CV_8UC3 );
  for( int i = 0; i< contours.size(); i++ )
     {
       Scalar color = Scalar( rng.uniform(0, 255), rng.uniform(0,255), rng.uniform(0,255) );
       // contour
       drawContours( drawing, contours, i, color, 1, 8, vector<Vec4i>(), 0, Point() );
       // ellipse
       ellipse( drawing, minEllipse[i], color, 2, 8 );
       // rotated rectangle
       Point2f rect_points[4]; minRect[i].points( rect_points );
       for( int j = 0; j < 4; j++ )
          line( drawing, rect_points[j], rect_points[(j+1)%4], color, 1, 8 );
     }
 
  /// 结果在窗体中显示
  namedWindow( "Contours", CV_WINDOW_AUTOSIZE );
  imshow( "Contours", drawing );
}</span>
```

### 12.图像轮廓的多边形逼近(approxPolyDP)

#### 12.1基本概念

findContours后的轮廓信息contours可能过于复杂不平滑，可以用approxPolyDP函数对该多边形曲线做适当近似，approxPolyDP 主要功能是把一个连续光滑曲线折线化，对图像轮廓点进行多边形拟合。

![img](https://img-blog.csdnimg.cn/a49d51712a204adfb4a4146655297339.gif)

####12.2API

approxPolyDP(   )

参数

InputArray curve:一般是由图像的轮廓点组成的点集

OutputArray approxCurve：表示输出的多边形点

double epsilon：主要表示输出的精度，就是各个轮廓点之间最大距离数，这个参数表示的是精度，越小精度越高，因为表示的意思是是原始曲线与近似曲线之间的最大距离

bool closed：表示输出的多边形是否封闭，若设为true，则认为曲线是闭合的，第一个点和最后一个点相连。

### 13.图像基本处理

#### 13.1计算轮廓周长或曲线长度（不适用于非轮廓的点集）

##### 13.1.1API

 double      arcLength( InputArray curve, bool closed );

参数

curve 曲线点集序列或数组 

closed 表示曲线是否闭合： closed=true- 假设曲线闭合 ，则最后一点到第一点的距离计入总弧长。函数 arcLength 通过依次计算序列点之间的线段长度，并求和来得到曲线的长度

#### 13.2获得矩形包围框（适合所有点集）

##### 13.2.1API

Rect boundingRect( InputArray points );

返回包覆输入信息的==最小正矩形==

参数

points：输入信息，可以为包含点的容器(vector)或是Mat。

#### 13.3获取最小包围圆

##### 13.3.1API

void minEnclosingCircle(  )

points：输入信息，可以为包含点的容器(vector)或是Mat。

center：包覆圆形的圆心。

radius：包覆圆形的半径。

#### 13.4获得轮廓的最佳拟合直线

##### 13.4.1API

void  fitline(   )

参数

第一个参数是用于拟合直线的输入点集，可以是二维点的cv::Mat数组，也可以是二维点的STL vector。

第二个参数是输出的直线，对于二维直线而言类型为cv::Vec4f，对于三维直线类型则是cv::Vec6f，输出参数的前半部分给出的是直线的方向，而后半部分给出的是直线上的一点（即通常所说的点斜式直线）。

第三个参数是距离类型，拟合直线时，要使输入点到拟合直线的距离和最小化（即下面公式中的cost最小化），可供选的距离类型如下表所示，ri表示的是输入的点到直线的距离。

![img](https://img-blog.csdnimg.cn/20190210142812846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMwODE1MjM3,size_16,color_FFFFFF,t_70)

第四个参数是距离参数，跟所选的距离类型有关，值可以设置为0，cv::fitLine()函数本身会自动选择最优化的值

第五、六两个参数用于表示拟合直线所需要的径向和角度精度，通常情况下两个值均被设定为1e-2

##### 13.4.2参考代码

```c++
	//创建一个用于绘制图像的空白图
	cv::Mat image = cv::Mat::zeros(480, 640, CV_8UC3);
 
	//输入拟合点
	std::vector<cv::Point> points;
	points.push_back(cv::Point(48, 58));
	points.push_back(cv::Point(105, 98));
	points.push_back(cv::Point(155, 160));
	points.push_back(cv::Point(212, 220));
	points.push_back(cv::Point(248, 260));
	points.push_back(cv::Point(320, 300));
	points.push_back(cv::Point(350, 360));
	points.push_back(cv::Point(412, 400));
 
	//将拟合点绘制到空白图上
	for (int i = 0; i < points.size(); i++)
	{
		cv::circle(image, points[i], 5, cv::Scalar(0, 0, 255), 2, 8, 0);
	}
 
	cv::Vec4f line_para; 
	cv::fitLine(points, line_para, cv::DIST_L2, 0, 1e-2, 1e-2);
 
	std::cout << "line_para = " << line_para << std::endl;
 
	//获取点斜式的点和斜率
	cv::Point point0;
	point0.x = line_para[2];
	point0.y = line_para[3];
 
	double k = line_para[1] / line_para[0];
 
	//计算直线的端点(y = k(x - x0) + y0)
	cv::Point point1, point2;
	point1.x = 0;
	point1.y = k * (0 - point0.x) + point0.y;
	point2.x = 640;
	point2.y = k * (640 - point0.x) + point0.y;
 
	cv::line(image, point1, point2, cv::Scalar(0, 255, 0), 2, 8, 0);
 
	cv::imshow("image", image);
	cv::waitKey(0);
	return;
```

#### 13.5计算整个轮廓或者部分轮廓的面积

##### 13.5.1API

double contourArea(    )

参数

contour:findcontour函数得到的轮廓；

oriented: 轮廓的方向影响面积的符号。因此函数也许会返回负的结果。应用函数 fabs() 得到面积的绝对值

#### 13.6测试点是否在多边形中

##### 13.6.1API

double   pointPolygonTest(  )

参数

contour：输入轮廓.

 pt针对轮廓需要测试的点

measure_dist：如果非0，函数将估算点到轮廓最近边的距离。

函数PointPolygonTest 决定测试点是否在轮廓内，轮廓外，还是轮廓的边上（或者共边的交点上），当measure_dist为false时，返回值对应是1, -1,0；当 measure_dist为true ，它返回一个从点到最近的边的带符号距离。

### 14.色彩识别

#### 14.1为什么选用HSV

HSV的三个系数中，H基本上就可以确定色相属于哪种颜色了，S是指图像的透明度，即颜色与白色混合程度；V是指图像的亮度，即颜色与黑色混合程度。而RGB由三个分量贡献，每种分量都能改变总颜色，使得颜色难以判断。

#### 14.2识别方法

基本原理很简单，读入图片后，首先转换成HSV颜色空间。然后逐一的判断每个像素是否在一定范围内，并标识出来（是就标识为255，不是就标识为0）（实际上这一步用了opencv里自带的API）。这样就可以用查找轮廓的方式，把每个颜色区域标识出来。

#### 14.3API

##### 14.3.1RGB转化为HSV

cvtColor(  image  ,  hsvimg  ,   COLOR_BGR2HSV)

##### 14.3.2查找相关颜色，并转化为二值图

inRange

参数

原图像  下限  上限   输出图像

解释：

inRange函数是将图像中不满足大于上限，小于下限的像素点变为0，满足条件的变为255，使得原图像变为二值图像。

![img](https://img-blog.csdnimg.cn/20190711211841353.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2tlbmV5cg==,size_16,color_FFFFFF,t_70)

#### 14.4参考代码

```c++
#include<opencv2/core.hpp>
#include<opencv2/highgui.hpp>
#include<opencv2/imgproc.hpp>
using namespace cv;
#include<iostream>
#include<string>
using namespace std;
//输入图像
Mat img;
//灰度值归一化
Mat bgr;
//HSV图像
Mat hsv;
//色相
int hmin = 0;
int hmin_Max = 360;
int hmax = 180;
int hmax_Max = 180;
//饱和度
int smin = 0;
int smin_Max = 255;
int smax = 255;
int smax_Max = 255;
//亮度
int vmin = 106;
int vmin_Max = 255;
int vmax = 255;
int vmax_Max = 255;
//显示原图的窗口
string windowName = "src";
//输出图像的显示窗口
string dstName = "dst";
//输出图像
Mat dst;
//回调函数
void callBack(int, void*)
{
	//输出图像分配内存
	dst = Mat::zeros(img.size(), img.type());
	//掩码
	Mat mask;
	inRange(hsv, Scalar(hmin, smin, vmin), Scalar(hmax, smax, vmax), mask);
	//掩模到原图的转换
	for (int r = 0; r < bgr.rows; r++)
	{
		for (int c = 0; c < bgr.cols; c++)
		{
			if (mask.at<uchar>(r, c) == 255)
			{
				dst.at<Vec3b>(r, c) = bgr.at<Vec3b>(r, c);
			}
		}
	}
	//输出图像
	imshow(dstName, dst);
	//保存图像
	//dst.convertTo(dst, CV_8UC3, 255.0, 0);
	imwrite("HSV_inRange.jpg", dst);
}
int main(int argc, char** argv)
{
	//输入图像
	img = imread("test.jpg");
	if (!img.data || img.channels() != 3)
		return -1;
	imshow(windowName, img);
	bgr = img.clone();
	//颜色空间转换
	cvtColor(bgr, hsv, CV_BGR2HSV);
	cout << hsv << endl;
	//定义输出图像的显示窗口
	namedWindow(dstName, WINDOW_GUI_EXPANDED);
	//调节色相 H
	createTrackbar("hmin", dstName, &hmin, hmin_Max, callBack);
	createTrackbar("hmax", dstName, &hmax, hmax_Max, callBack);
	//调节饱和度 S
	createTrackbar("smin", dstName, &smin, smin_Max, callBack);
	createTrackbar("smax", dstName, &smax, smax_Max, callBack);
	//调节亮度 V
	createTrackbar("vmin", dstName, &vmin, vmin_Max, callBack);
	createTrackbar("vmax", dstName, &vmax, vmax_Max, callBack);
	callBack(0, 0);
	waitKey(0);
	return 0;
}
```

#### 14.5详细步骤

##### 14.5.1指定颜色范围

```c++
enum colorType{Red = 0, Green, Blue, ColorButt};
 
const Scalar hsvRedLo( 0,  40,  40);
const Scalar hsvRedHi(40, 255, 255);
 
const Scalar hsvGreenLo(41,  40,  40);
const Scalar hsvGreenHi(90, 255, 255);
 
const Scalar hsvBlueLo(100,  40,  40);
const Scalar hsvBlueHi(140, 255, 255);
 
vector<Scalar> hsvLo{hsvRedLo, hsvGreenLo, hsvBlueLo};
vector<Scalar> hsvHi{hsvRedHi, hsvGreenHi, hsvBlueHi};
 
vector<String> textColor{"R", "G", "B"};
```

##### 14.5.2查找相关颜色并转化为二值图

```c++
        // 查找指定范围内的颜色
        inRange(hsvImg, hsvLo[colorIdx], hsvHi[colorIdx], imgThresholded);
```

##### 14.5.3将所得的二值图像四边加个像素，这样做可避免纯色图片无边框导致无法识别。

```c++
copyMakeBorder(imgThresholded, imag_1, 1, 1, 1, 1, BORDER_CONSTANT, 0);
vector<vector<Point> > contours0;
vector<Vec4i> hierarchy;
findContours(imag_1, contours0, hierarchy, RETR_TREE, CHAIN_APPROX_SIMPLE);
```

##### 14.5.4检查所有轮廓中心，如果非0，判断为所需要查找的颜色区域，用文字记录。

```c++
for (int idx =0; idx < contours0.size(); idx++ )
{
Rect bound = boundingRect(Mat(contours0[idx]));
Point bc = Point(bound.x + bound.width / 2,
bound.y + bound.height / 2);
uchar x = imgThresholded.at<uchar>(bc);
 if (x > 0)
      {
     org = bc;
 putText(image, textColor[colorIdx], org, fontFace, 1, color,thickness, lineType, bottomLeftOrigin);
 }
  }
```

![img](https://img-blog.csdn.net/20160526140204634?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)





###15文字与矩形绘制

#### 15.1文字

##### 15.1.1API

cv2.putText

参数

图片，添加的文字，左下角坐标，字体，字体大小(数值越大，字体越大,可以为小数)，颜色，字体粗细(越大越粗)

### 16.Canny函数及使用

Canny 边缘检测是一种使用多级边缘检测算法检测边缘的方法。

#### 16.1测量步骤

Canny 边缘检测分为如下几个步骤：
步骤 1：去噪。噪声会影响边缘检测的准确性，因此首先要将噪声过滤掉。
步骤 2：计算梯度的幅度与方向。
步骤 3：非极大值抑制，即适当地让边缘“变瘦”。
步骤 4：确定边缘。使用双阈值算法确定最终的边缘信息。

#### 16.2API

img   Canny

参数

会返回计算得到的边缘图像

image 为 8 位输入图像。

 threshold1 表示处理过程中的第一个阈值。

threshold2 表示处理过程中的第二个阈值。

apertureSize 表示 Sobel 算子的孔径大小。

L2gradient 为计算图像梯度幅度（gradient magnitude）的标识。其默认值为 False。如果为 True，则使用更精确的 L2 范数进行计算（即两个方向的导数的平方和再开方），否则使用 L1 范数（直接将两个方向导数的绝对值相加）。

#### 16.3参考代码

```c++
import cv2
o=cv2.imread("lena.bmp",cv2.IMREAD_GRAYSCALE)
r1=cv2.Canny(o,128,200)
r2=cv2.Canny(o,32,128)
cv2.imshow("original",o)
cv2.imshow("result1",r1)
cv2.imshow("result2",r2)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 17.HighGUI

#### 17.1图像的处理

##### 17.1.1图像的载入

imread( onst string& filename, int  flags )

flags取值：

IMREAD_COLOR	读取三通道图像，即使输入是灰度图像，也会有三通道，只是每个通道拥有相同的数据，是默认选项。
IMREAD_GRAYSCALE      读取单通道图像	
IMREAD_ANYCOLOR	 通道数由文件实际通道数（不超过3）
cIMREAD_ANYDEPTH	允许加载超过8bit深度	
IMREAD_UNCHANGED	相当于IMREAD_ANYCOLOR和IMREAD_ANYDEPTH组合使用，可保留alpha通道。

##### 17.1.2图像的载入

imwrite( place+filename,  image,  params )

常用扩展名 :

.jpg或.jpeg    8位	单通道或三通道
.jp2		8位或16位	单通道或三通道
.tif或.tiff		8位或16位	单通道、三通道或四通道
.png		8位或16位	单通道、三通道或四通道
.bmp		8位	单通道、三通道或四通道
.ppm或.pgm	8位	单通道（PGM）和三通道（PPM）

params的使用 

| 标志                        | 含义                                | 取值范围 | 默认值 |
| --------------------------- | ----------------------------------- | -------- | ------ |
| cv::IMWRITE_JPG_QUALITY     | JPEG的质量                          | 0~100    | 95     |
| cv::IMWRITE_PNG_COMPRESSION | PNG压缩值                           | 0~9      | 3      |
| cv::IMWRITE_PXM_BINARY      | 对PPM、PGM或PBM文件是否使用二值格式 | 0或1     | 1      |

##### 17.1.3图像的编码

void imencode(   )

| 参数   | 含义                                                         |
| ------ | ------------------------------------------------------------ |
| ext    | 文件扩展名，用于函数与对应的编码压缩方式相关联。指明格式并帮助系统索引对应编码库 |
| img    | 要被压缩的图像数据                                           |
| buf    | 用来存储编码压缩后的图像数据的字符数组                       |
| params | 指明要使用编码器的输入参数                                   |

##### 17.1.4解码文件

imdecode(InputArray buf, int flags=cv::IMREAD_COLOR)

#### 17.2视频处理

##### 17.2.1读取视频流

VideoCapture(const string& filename)

VideoCapture(int device)

VideoCapture()

作用：打开视频文件或摄像机。打开成功则cv::VideoCapture::isOpen()会返回true，否则返回false。注：最后一种形式只是直接创建一个VideoCapture对象，无法直接使用，需要调用cv::VideoCapture::open()函数来指定文件名或设备号，和前两种形式一样。

构造设备参数时，传入的标识符是相机的域索引和相机索引之和。相机索引是指系统中摄像机设备的索引，从0开始计数。相机域表明我们用何种相机，如下表所示。

相机捕捉常数	数值	

cv::CAP_ANY	 0    	cv::CAP_DSHOW	700	cv::CAP_OPENNI2	1600
cv::CAP_MIL	100	cv::CAP_PVAPI	800	cv::CAP_OPENNI2_ASUS	1610
cv::CAP_VFW	200	cv::CAP_OPENNI	900	cv::CAP_GPHOTO2	1700
cv::CAP_V4L	200	cv::CAP_OPENNI_ASUS	910	cv::CAP_GSTREAMER	1800
cv::CAP_V4L2	200	cv::CAP_ANDROID	1000	cv::CAP_FFMPEG	1900
cv::CAP_FIREWIRE	300	cv::CAP_XIAPI	1100	cv::CAP_IMAGES	2000
cv::CAP_FIREWARE	300	cv::CAP_AVFOUNDATION	1200	   cv::CAP_ARAVIS	2100
cv::CAP_IEEE11394	300	cv::CAP_GIGANETIX	1300	cv::CAP_OPENCV_MJPEG	2200
cv::CAP_DC1394	300	cv::CAP_MSMF	1400	cv::CAP_INTEL_MFX	2300
cv::CAP_QT	500	cv::CAP_WINRT	1410	cv::CAP_XINE	2400
cv::CAP_UNICAP	600	cv::CAP_INTELPERC	1500	 	 

当传入的标识符为-1时，则OpenCV会弹出一个选择窗口，可手动选择希望使用的相机。

##### 17.2.2从视频流中读取图像

**使用cv::VideoCapture::read()从视频流中读取图像**

bool cv::VideoCapture::read(OutputArray img)

作用：从文件中读取下一帧数据，并将结果存入img中，并会自动更新。读取成功返回true，读取失败返回false。

**使用cv::VideoCapture::operator>>()从视频流读取图像数据**

cv::VideoCapture& cv::VideoCapture::operator>>(Mat& img)

作用：重载输入流操作符，从文件中读取下一帧数据，并将结果存入img中。

**使用cv::VideoCapture::grab()和cv::VideoCapture::retrieve()从视频流中读取图像数据**

bool cv::VideoCapture::grab()

作用：将当前可使用的图像数据拷贝至用户看不到的一个内存缓存区。抓取的图像帧是未处理的，该函数只是设计用于将原始数据尽快获取到电脑。

bool cv::VideoCapture::retrieve(OutputArray image, int channel)

作用：对内部缓存数据进行必要的图像解码、内存分配与拷贝，最后以Mat形式的序列将图像帧返回。

##### 17.2.3相机属性

**相机属性cv::VideoCapture::get()和cv::VideoCapture::set()**

double cv::VideoCapture::get(int propid)

bool cv::VideoCapture::set(int propid, double value)

作用：获取或设置元数据（meta data）。

##### 17.2.4写入视频

cv::VideoWriter::VideoWriter(const string& filename, int fourcc, double fps, Size frame_size, bool is_color=true)

cv::VideoWriter::VideoWriter()

作用：创建一个VideoWriter对象，is_color为true则传入彩色图，反之可以传入灰度图。同样第二种形式不能直接使用，需要调用cv::VideoWriter::open()函数进行配置。

判断是否初始化成功，可通过cv::VideoWriter::isOpen()的返回值进行判断。

**使用cv::VideoWriter::write()函数完成图像帧的写入**

cv::VideoWriter::write(const Mat& img)

作用：完成图像帧的写入，图像必须和cv::VideoWriter具有相同的图像尺寸，如果是彩色必是三通道，反之使用单通道。

**使用cv::VideoWriter::operator<<()将图像写入视频**

cv::VideoWriter& cv::VideoWriter::operator<<(Mat& img)

作用：重载输出流操作符，将img存入视频文件。

### 18.对彩色图像进行均衡化处理

#### 18.1概念

在图像识别的过程中，**增加灰度对比度可以突出图像重要的特征**，直方图均衡化是通过改变每个灰度级上像素点分布，使其都具有相同的象素点数，目的是**使图像在整个灰度值动态变化范围内分布均匀化，改善图像的亮度分布状态，增强图像的视觉效果**。

#### 18.2通道分离

```c++
void cv::split(inputArray, src)  //多通道数组
vector<>channels  //把多个数组放到vector容器中
//图像通道分离
```

#### 18.3灰度图均值化

```c++
void cv::equalizeHist ( InputArray src,  //输入图像
OutputArray dst           //输出图像
) 
//注意：该函数只支持单通道的灰度图均衡化，所以对于彩色图像来说可以先将多通道分离成单通道，然后再合并成多通道。
```

####18.4通道合并

```c++
void cv::merge(vector<>channels  //待混合的容器
inputArray, dst ） //混合后的数组
```

#### 18.5参考代码

```c++
#include <iostream>
#include <string>
#include<cctype>
#include <fstream>
#include<vector>
#include "opencv2/core/core.hpp"
#include "opencv2/imgproc/imgproc.hpp"
#include "opencv2/calib3d/calib3d.hpp"
#include "opencv2/highgui/highgui.hpp"
using namespace cv;
using namespace std;
int main()
{
	Mat m = imread("D:\\My code\\Python\\opencv\\study\\01.jpg");
	vector<Mat> channels;//定义存储的容器
	split(m, channels);
	Mat dst;
	Mat bluechannel = channels[0];//b通道的图像
	equalizeHist(bluechannel, bluechannel);//均衡化
	Mat greenchannel = channels[1];//g通道的图像
	equalizeHist(greenchannel, greenchannel);
	Mat redchannel = channels[2];//r通道的图像
	equalizeHist(redchannel, redchannel);

	merge(channels, dst);//合并通道
	imshow("before",m);
	imshow("after",dst);
	waitKey(0);
	system("pause");
}
```

### 19矩阵

#### 19.1Rect

```c++
Rect rect（x,y,_width,height） // 注意后面两个数值
//一个image: _width=image.cols height=image.height 
rect.area();   //返回矩形面积
rect.tl();     // top-left corner, 返回值为Point_<Tp>类型,返回rect左上角坐标
rect.br();     // bottom-right corner , 返回值为Point_<Tp>类型
rect.contains(Point(x, y));  //Rect 是否包含Point ,返回bool类型


recta & rectb  //求交集,经常用来防止访问溢出 

```

#### 19.2RotatedRect

```c++
//构建一个Mat（200*200）
Mat image(200, 200, CV_8UC3, Scalar(0));  
//设置一个旋转矩形3个参数分别为：质心（矩形中心），矩形长宽100、50 旋转角度：30 （clockwise）
//RotatedRect 函数返回一个旋转矩形对象
RotatedRect rRect = RotatedRect(Point2f(100,100), Size2f(100,50), 30);

Point2f vertices[4];      //定义4个点的数组
rRect.points(vertices);   //将四个点存储到vertices数组中
for (int i = 0; i < 4; i++)
    // 注意Scala中存储顺序 BGR
    line(image, vertices[i], vertices[(i+1)%4], Scalar(0,255,0));
// 返回外接矩形
Rect brect = rRect.boundingRect();
rectangle(image, brect, Scalar(255,0,0));
imshow("rectangles", image);
waitKey(0);
```

### 20.YAML与XML文件的打开和关闭

#### 20.1文件开关

#####20.1.1文件的打开

在Opencv中，使用FileStorage进行文件读写。XML文件操作与YAML一样，不过存在一些细小区别。

```c++
std::string fileName  = "E:\\test.yml"; // YAML
std::string fileName2 = "E:\\test.xml"; // XML
// write file
cv::FileStorage fs(fileName , cv::FileStorage::WRITE);
// read file
cv::FileStorage fs2(fileName , cv::FileStorage::READ);
// or use: cv::FileStorage::open
fs2.open(fileName , cv::FileStorage::READ);
```

FileStorage的文件操作一共分为4种：

READ，WRITE，APPEND(追加），MEMORY.

##### 20.1.2判断文件打开是否成功

```c++
if ( !fs.isOpened() ) // failed
{
    std::cout<<"Save File Failed!"<<std::endl;
    return ;
}
```

##### 20.1.3文件的关闭

```c++
fs.release();
```

#### 20.2文件的读写

FileStorage文件读与写的方法与C++语言中的文件流对象的使用很像，对>>和<<进行了重载，分别用于文件读取和写入。很棒的是，`FileStorage`支持一些常用格式的直接读写，例如字符、字符串、数字、`cv::Mat`等。对于不支持的数据结构，只能按照规则自己去写啦~

##### 20.2.1写入

```c++
fs << "frameCount" << 5;  // 字符和数字
cv::Mat_<double> cameraMat = cv::Mat_<double>::zeros(3, 3); 
fs << "Camera Intrinsic Matrix" << cameraMat; // cv::Mat
```

##### 20.2.2读取

读取方式有两种

```c++
// 第一种
int frameCount = (int)fs2["frameCount"];
// 第二种
int frameCount;
fs2["frameCount"] >> frameCount;
```

##### 20.2.3Mat的操作

```c++
cv::Mat_<double> cameraMat = cv::Mat_<double>::zeros(3, 3);
cv::Mat_<double> distCoeffes = ( cv::Mat_<double>(5, 1)<< 0.1, 0.01, -0.001, 0.0, 0.0 );
// C++
std::cout<<"Camera Matrix"<<std::endl<<cv::Mat::Mat(cameraMat)<<std::endl;
std::cout<<"Distortion Coefficients"<<std::endl<<cv::Mat::Mat(distCoeffes)<<std::endl;
// cv::FileStorage
fs << "Camera Matrix" << cameraMat;
fs << "Distortion Coefficients"<<distCoeffes;
```

##### 20.2.4集合的操作

###### 20.2.4.1映射集合

即每个元素都有自己的名字，可通过名字进行访问。

```c++
// Mappings write
int x(1.0), y(0.0);
fs << "features" << "["; // also can be "[:"
fs <<"{:" << "x" << x << "y" << "}" << "]";
```

注意:     “{”和“{:”`输出的结果是不一样的，YAML使用`":"`后，使输出的文本具有Python的风格，映射集合会按照一行排列，不适用时，按照每个元素与其值单独一行的方法排列。XML使用`":"`后输出结果会有不同，但基本可以视为把`":"`忽略。

读取

```c++
// Mappings read
cv::FileNode features = fs2["features"];
// 遍历查看
cv::FileNodeIterator it = features.begin();
std::cout<<
    "x="<<(int)(*it)["x"]<<
    " y="<<(int)(*it)["y"]<<
    " z="<<(int)(*it)["z"]<<std::endl;
```

###### 20.2.4.2序列集合   

数据没有名字，一般通过序号访问。

```c++
// Sequences write
int mySeq[5] = {0, 1, 2, 3, 4};
fs << "mySeq" << "[";
for ( int idx=0; idx<5; idx++ )
{
    fs << mySeq[idx];
}
fs << "]";
```

```c++
// Sequences read
cv::FileNode mySeq2 = fs2["mySeq"];
std::vector<int> seq;
cv::FileNodeIterator it = mySeq2.begin(), it_end = mySeq2.end();
for ( ; it != it_end; it++  )
{
    seq.push_back( (int)( *it ) );
    // std::cout<<(int)(*it)<<" "<<std::endl;
}
```

### 21. findcounters

#### 21.1API

```c++
findContours( InputOutputArray image, OutputArrayOfArrays contours,
                              OutputArray hierarchy, int mode,
                              int method, Point offset=Point());
```

第一个参数**：image，单通道图像矩阵，**可以是灰度图，但更常用的是二值图像，一般是经过Canny、拉普拉斯等边缘检测算子处理过的**二值图像**；

第二个参数：contours，定义为“==vector<vector<Point\>> contours==”，是一个向量，并且是一个双重向量，向量内**每个元素保存了一组由连续的Point点构成的点的集合的向量，每一组Point点集就是一个轮廓**。  有多少轮廓，向量contours就有多少元素。

第三个参数：hierarchy，定义为“==vector<Vec4i\> hierarchy==”，先来看一下Vec4i的定义： typedef    Vec<int, 4>   Vec4i;   Vec4i是Vec<int,4>的别名，定义了一个“向量内每一个元素包含了4个int型变量”的向量。所以从定义上看，hierarchy也是一个向量，**向量内每个元素保存了一个包含4个int整型的数组。** 向量hiararchy内的元素和轮廓向量contours内的元素是一一对应的，向量的容量相同。 **hierarchy向量内每一个元素的4个int型变量——hierarchy[i\][0] ~hierarchy[i\][3]，分别表示第i个轮廓的后一个轮廓、前一个轮廓、父轮廓、内嵌轮廓的索引编号。**如果当前轮廓没有对应的后一个 轮廓、前一个轮廓、父轮廓或内嵌轮廓的话，则hierarchy[i][0] ~hierarchy[i][3]的相应位被设置为默认值-1；

第四个参数：int型的mode，定义轮廓的==检索模式==：

取值一：CV_RETR_EXTERNAL**只检测最外围轮廓**，包含在外围轮廓内的内围轮廓被忽略

取值二：CV_RETR_LIST  检测所有的轮廓，包括内围、外围轮廓，但是检测到的轮廓不建立等级关系，彼此之间独立，没有等级关系，这就意味着**这个检索模式下不存在父轮廓或内嵌轮廓**，所以hierarchy向量内所有元素的第3、第4个分量都会被置为-1。

取值三：CV_RETR_CCOMP  检测所有的轮廓，但所有轮廓只建立两个等级关系，外围为顶层，若外围内的内围轮廓还包含了其他的轮廓信息，则内围内的所有轮廓均归属于顶层

取值四：CV_RETR_TREE， 检测所有轮廓，所有轮廓建立一个等级树结构。外层轮廓包含内层轮廓，内层轮廓还可以继续包含内嵌轮廓。

第五个参数：int型的method，定义轮廓的近似方法：

取值一：CV_CHAIN_APPROX_NONE 保存物体边界上所有连续的轮廓点到contours向量内

取值二：CV_CHAIN_APPROX_SIMPLE **仅保存轮廓的拐点信息**，把所有轮廓拐点处的点保存入contours向量内，拐点与拐点之间直线段上的信息点不予保留

取值三和四：CV_CHAIN_APPROX_TC89_L1，CV_CHAIN_APPROX_TC89_KCOS使用teh-Chinl chain 近似算法

第六个参数：Point偏移量，所有的轮廓信息相对于原始图像对应点的偏移量，**相当于在每一个检测出的轮廓点上加上该偏移量**，并且Point还可以是负值！



### 22.atan2

double atan2(double y,double x) 

**返回的是原点至点(x,y)的方位角，即与 x 轴的夹角。****

返回值的单位为弧度，取值范围为（-π, π]。结果为正表示从 X 轴逆时针旋转的角度，结果为负表示从 X 轴顺时针旋转的角度。若要用度表示反正切值，请将结果再乘以 180/π。另外要注意的是，函数atan2(y,x)中参数的顺序是倒置的，atan2(y,x)计算的值相当于点(x,y)的角度值。





































