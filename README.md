# Motion-Detection<br>
Motion Detection Algorithm
***
## 31 March
### 空间物体检测
- **背景更新周期** 间隔一段时间周期,把当前帧更新为背景帧,这样做的好处是提高了算法的运行速度,而且可以把进入场景后静止的物体快速降级为背景
- **是否有运动物体** 计算同各个点在前景帧和背景帧之间的差值和,判断是否出现了运动目标,该方法适用于**运动物体较少的场景**<br>
> **算法的实现过程描述如下:**
> -  从 CCD 像机中取得第 1 帧图像作为背景帧,把该帧图像二值化.
> -  从 CCD 像机中取得第 2 帧图像作为前景图像,把该帧图像二值化.
> -  依次取出前景图中第 k 个像素点的像素值 q (i , j) 背景图中的第 k 个像素点的像素值 b (i , j)把这两个像素值相减后取绝对值,然后计算所有绝对值之和 如果绝对值之和大于阈值 t, 则说明前两张图像差别较大,判断为出现了空间运动目标;如果绝对值之和小于阈值t,说明前后两张图像无显著变化,可判断为场景中没有出现运动物体.
> -  周期到达时,把当前帧更新为背景帧,取得下一帧图像作为前景帧,返回上一步
### 场景变化程度分级
- **图像变化程度**  根据变化程度采用不同的算法,如对于光线变化程度较大,可以采用**帧间差法**

<div align=center><img src="http://latex.codecogs.com/gif.latex?\sum_{x}\sum_{y}{|f_{current}(x,y)-f_{previous}(x,y)|}"> </img></div>

- **阈值** 根据变化强度,适当选择阈值,计算时可以采用**达到阈值即可退出**的方法
### 基于区块特征选择然后再特征融合
- **算法目的** 便于追踪行人，提高辨识精度
> **算法实现:**
> - **特征选择** 有意识的将person按照结构（structure）划分成若干个区域（利用mean shift聚类等算法）如上身和下肢，走路时上身不动，但是下肢运动
> - **特征融合** 不同区域特征进行融合，重新判断（即进行re-id操作）
- **结果：** 提高了分辨精度，便于人物追踪
### 分块思想
- 针对大部分video/动图，大部分移动的物体仅仅是占据图片的一个区域，如果只对**该区域**及其**周围部分区域**采用背景建模算法，可以进一步提升运算速度。再可以试着利用机器学习进行预测。
## 3 April
### LBP算子（除阴影） 
- 差值二值化，阴影部分像素灰度值整体降低
### 动态信息窗口记录
- 均值、方差、最近N个采样值、前景点比率等
### 加速因子
- 分级更新模型
### 全局模型
- 前景点概率与背景点概率，通过一个函数确定当前点为背景点还是前景点
### 像素点统计规律
- 背景点符合RGB空间3维线性
### 三帧差法除阴影
- 三个图像帧做差

<div align=center><img src="http://latex.codecogs.com/gif.latex?\\\left\{\begin{matrix}D_{k}=F_{k}-F_{k-1}\\D_{k-1}=F_{k-1}-F_{k-2}\end{matrix}\right."> </img></div>

- 取差值交集

<div align=center><img src="http://latex.codecogs.com/gif.latex?M_{k-1}=D_{k}\cap%20D_{k-1}"> </img></div>

- 最终结果:


<div align=center><img src="http://latex.codecogs.com/gif.latex?M_{k}=D_{k}-M_{k-1}"> </img></div>

### 背景模型建立与更新
- 中值法

<div align=center><img src="http://latex.codecogs.com/gif.latex?B(x,y)=median[I_0(x,y),I_1(x,y)...I_N-1(x,y)]"> </img></div>

-更新方法

<div align=center><img src="http://latex.codecogs.com/gif.latex?B_n(x,y)=\alpha%20\cdot%20B_{n-1}(x,y)+(1-\alpha%20)\cdot%20I_n(x,y)">
