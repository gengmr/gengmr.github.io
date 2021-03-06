---
title: 我所理解的图像处理技术栈
date: 2017-03-19 21:19:03
categories:
 - 图像处理
tags:
 - 图像识别
 - 图像处理
 - 计算机视觉
---

图像处理，是对图像进行分析、加工、和处理，使其满足视觉、心理以及其他要求的技术。图像处理是信号处理在图像域上的一个应用。

# 基本图像处理
## 图像产生
这篇文章所讨论的图像为2D图像。目前图像主要有2类来源：
1. 摄像机等硬件设备收集自然场景中产生的信号（如光学信号，红外信号等）所形成的图像；
2. 由算法生成的图像，如图形学技术产生的游戏画面等，或者类似GAN（Generative Adversarial Networks）产生的图像。

## 图像表示
图像进行数字存储和处理需要表示方式，目前多采用下列颜色空间进行表示。

### RGB
最常用的图像表示是以RGB为颜色空间的图像表示。RGB分别表示Red、Green、Blue，以三原色作为颜色的基础。并且有时候会附加Alpha通道，表示透明程度。
![](./RGB.png)

### YUV/YCbCr
视频领域最长使用的图像表示方法。Y、UV分别表示亮度和色度，色度和亮度分离主要是早期彩色电视用于兼容黑白电视。YUV空间又名YCbCr空间，Cb和Cr分别表示红蓝的色度。
![](./YUV.png)

### CMY/CMYK
CMY/CMYK颜色空间通常用于印刷出版。CMYK分别表示Cyan（青）、Magenta（品红）、Yellow（黄色）、BlacK（黑色）。该颜色空间主要在于CMYK四种颜色在实际场景中属于“三原色”，能很方便的制取获得。
![](./CMYK.png)

### HSL/HSV
HSL/HSV在很多需要用户进行颜色调整的场景，如视频编辑软件、图像编辑软件等。H代表Hue（色相），S达标Saturation（饱和度），L代表Luminance（亮度），V代表Value（明度）。  
![](./HSL&HSV.png)

### 其他
除了上述4种颜色空间之外，有时为了特殊用途，也有一些其他的颜色空间，更多见：[List of Color Space And Their Uses](https://en.wikipedia.org/wiki/List_of_color_spaces_and_their_uses)

## 图像压缩
图像压缩是指将图像通过某种压缩算法进行压缩存储和传输。一般压缩算法分为有损和无损压缩，前者代表性算法是png，后者代表性算法是jpg。

## 图像变换
图像操作包括一些基础的图像变换技术，如下。

### 算数变换
图像的算数变换是指对图像作算数变换，包括：
1. 图像相加
2. 图像相减

### 逻辑变换
图像的逻辑变换是指对图像作逻辑变换，包括：
1. 图像And操作
2. 图像Or操作
3. 图像Not操作

### 几何变换
图像的几何变换是指对图像作几何操作，包括：
1. 平移
> 指对图像进行X/Y方向的移动操作，一般移动后产生的空白填充为背景色。
2. 翻转
> 翻转是指对图像进行水平或者竖直方向的镜像处理。
3. 旋转
> 旋转是指对图像围绕某个中心点进行旋转操作，旋转之后图像宽高会产生变化，旋转后产生的空白一般填充为背景色，宽高变化主要有2种处理方式：裁剪、扩大。
4. 缩放
> 顾名思义，对图像进行放大或者缩小处理。
5.  透视
> 透视变换是指将图像投影到一个新的视平面，也称作投影映射。通常2D图像透视将会导致图像畸变，原始的长方形图像变换为不规则四边形，通过定位该不规则四边形的4个角点，可对图像进行透视纠正还原为原始图像。
6. 图像拼接
> 图像拼接是指将若干幅图像进行拼接，形成一张大图所做的操作。
7. 图像裁剪
> 图像裁剪是指分理出图像感兴趣区域（ROI）所做的操作。

### 颜色处理
颜色处理是指处理图像的颜色，包括：
1. 颜色变换
> 颜色变换是指各个颜色空间的转换以及某颜色空间中单通道的抽取或者多通道的合并等。在OpenCV中，通常表示为：RGB2HSV、GRAY2RGB、RGBA2GRAY等，更多见：[](http://docs.opencv.org/3.2.0/d7/d1b/group__imgproc__misc.html#ga4e0972be5de079fed4e3a10e24ef5ef0)。  
> 不同的颜色空间在不同的场景下会有不同的方便，如HSV空间进行颜色突出物体的提取比较好，因此在部分场景下需要先确定使用何种颜色空间。
2. 亮度、色度调整
> 通过调整颜色空间中的通道数值，进而调整整个图像的颜色表现。  

### 灰度变换
灰度变换是指对图像单通道进行颜色上的处理，包括：
1. 反相
> 即对图像进行颜色的翻转，原先的白色变成黑色，黑色变成白色。
2. 对数变换
> d=c * log(1+s)，对数变换是指原始像素灰度值做对数变换后乘以常数。对数变换使得图像能更好的显示灰度值较低的区域，一般傅里叶变换所产生的图像会做一下对数变换。
3. 伽马变换
> 伽马变换主要用于图像的颜色校正，将太亮或者太暗的图像进行修正。
4. 灰度拉伸
> 灰度拉伸是指改变图像的灰度范围，并对原始灰度映射到新的灰度范围。最简单是的线性拉伸，以及其他的非线性拉伸或者分段式拉伸等。
5. 比特平面分层
> 比特平面分成是指将灰度值分为多个bit平面，各个bit平面可能表示图像的不同域。
6. 灰度分层
> 灰度分层是指将图像某个灰度范围内的像素置为某确定灰度，使得该绘图范围的所有像素突出显示。
7. 直方图均衡化 
> 直方图是图像各个灰阶的像素数目的直方图统计，直方图均衡化能很好地解决低对比度等问题。直方图均衡化分为全局直方图均衡化和局部直方图均衡化。

### 形态学处理
图像形态学处理用于图像的结构提取，基本的运算包括：二值腐蚀和膨胀、二值开闭运算、骨架抽取、极限腐蚀、击中击不中变换、形态学梯度、Top-hat变换、颗粒分析、流域变换等（参考：[【OpenCV入门教程之十】 形态学图像处理（一）：膨胀与腐蚀](http://blog.csdn.net/poem_qianmo/article/details/23710721)。
1. 腐蚀
> 腐蚀操作是指在核（kernel）范围内，除非图像上所有点均为前景色，否则将核范围内所有像素置为背景色。一般用于图像的噪声消除等。
2. 膨胀
> 膨胀操作是指在核（kernel）范围内，如若图像上有任意点为前景色，将核范围内所有像素置为前景色。一般用于图像的断裂连接等。
3. 开运算
> 开运算是指先做腐蚀然后做膨胀。
4. 闭运算
> 闭运算是指先做膨胀然后做腐蚀。
5. 击中击不中
> 采用2个不相交的核，核1对原图做腐蚀操作，核2对核1腐蚀过后的图像的反相做腐蚀操作，最后的图像即为击中击不中的结果。
6. 连通域提取
> 连通域提取是图像领域最基础通用的算法之一，主要用于目标分割。
7. 细化
> 细化是指保留图像形状的基础上对图像进行细化的操作。
8. 骨架提取
> 骨架提取是只保留图像的基础骨架。
9. 粗化
> 粗化是指保留图像形状的基础上对图像进行粗化的操作。
10. 空洞填充
> 孔洞填充是用前景色指填充所有图像中的孔洞。

### 空间滤波
空间滤波是指采用某个核对图像作空间滤波操作，根据核的不同，也会产生不同的滤波效果。包括：
1. 平滑
> 平滑所采用的核包括中值、均值、高斯、双边以及其他变式核，主要是使得周围像素附加对中心像素产生影响，平滑掉突出像素。
2. 锐化
> 锐化主要用于强化图像的边缘地区，图像像素变化产生的一阶微分会产生较粗的边缘线，因此一般采用二阶微分作为锐化模板，如拉普拉斯算子等。
3. 边缘提取
> 边缘提取是指提取图像中边缘部分，主要有Canny算子等。

### 频率域滤波
频率域滤波不同于空间滤波，这种处理方式不是很直观，需要将图像转换为频率域的表示，然后在频率域进行滤波操作，最后还原为原始空域图像。在空间域中不好处理的图像，可能在频率域处理会比较方便。   
频域的高频决定细节，低频决定总体表现。高通滤波有利于提取图像轮廓和边缘，低通滤波有利于平滑图像噪声。频域滤波还可以去除比较规律性的周期噪声。更多图像频域滤波见：[图像频域滤波与傅里叶变换](http://blog.csdn.net/on2way/article/details/46981825)。  
频率域滤波主要依赖于傅里叶变换，傅里叶变换所产生的频谱图是观察频域的主要方式，频谱图对于图像的平移不敏感，随图像的旋转而旋转，频谱图的详细解释见：[图像的二维傅里叶变换频谱图特点研究](http://www.cnblogs.com/xh6300/p/5956503.html)。  
![](./frequency_domain.png)

## 图像处理进阶
图像处理进阶技术是指结合各基本处理技术进行综合，满足特定条件下的需求所产生的一些技术。

### 图像金字塔
图像金字塔是一种以多尺度角度处理图像的技术，在关心整体图像不关心细节时可直接处理金字塔顶层图像，并且在很多方法中会融合不同尺度所产生的信息。

### 霍夫曼变换
霍夫曼变换是一种转换视角进行问题分析的方法，在检测诸如直线和圆等规律性形状时，首先将特定形状参数化表示，然后根据图像中所有像素对参数的贡献估计适用的参数，进而估计出目标形状区域。

### 全局特征
图像全局特征是描述图像的整体特点的特征，包括颜色直方图特征、纹理特征等。可用于图像匹配与图像分析。

### 局部特征
图像局部特征是指描述图像局部特点的特征。其核心要素在于不变性和可区分性，不变性包括灰度不变性、尺度不变性、旋转不变性等，可区分性则是指特征很容易进行匹配，二者往往是负相关。  
常见的图像局部特征包括：
1. Harris角点
> 角点常用于图像匹配等场景，Harris角点检测方法是角点检测常用的一种方法。
2. SIFT特征
> SIFT特征具有较好的尺度、平移、旋转、亮度不变性，缺点是速度较慢。
3. SURF特征
> SURF特征较之SIFT特征，速度有一定提升。
4. ORB特征
> ORB特征运行速度远快于SIFT和SURF特征。
5. HOG特征
> 方向梯度直方图（Histogram of Oriented Gradient, HOG）特征是一种在计算机视觉和图像处理中用来进行物体检测的特征描述子。它通过计算和统计图像局部区域的梯度方向直方图来构成特征。常用于行人检测。
6. LBP特征
> LBP（Local Binary Pattern，局部二值模式）是一种用来描述图像局部纹理特征的算子；它具有旋转不变性和灰度不变性等显著的优点。LBP特征常用于人脸识别与表情识别。
7. haar特征
> Haar-like特征最早是由Papageorgiou等应用于人脸表示，Viola和Jones在此基础上，使用3种类型4种形式的特征。haar特征结合adaboost常用于人脸检测。

## 图像处理技术的应用
图像处理技术在实际场景中有非常多的应用，包括不限于如下：
### 图像增强
### 目标检测
### 目标识别
### 自动上妆
### etc.

未完待续。-> 图像识别。
