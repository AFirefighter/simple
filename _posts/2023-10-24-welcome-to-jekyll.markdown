---
layout: post
title:  "图形学入门"
date:   2023-10-24 23:02:32 +0800
categories: jekyll update
---
这是一个图形学入门笔记，记录图形学（包括渲染与仿真）基础知识。

# Game_103 学习笔记

## 首次学习

### Math Background:Vector, Matrix and Tensor Calculus.

==Vector==:Dot product and Cross prouct.(点乘和叉乘)。这都是很久以前就学习过的内容，使用它们可以来研究三维空间几何体的性质。

`叉乘`的作用举例，判断点在三角形的内部还是外部（二维情形）。对三条边都计算一下这个值，全大于0那么就在三角形内部。不过n法向量使用同一顶点出发的边向量计算，不要反过来。
$$
for \ example, \ n = \frac{x_{20}*x_{21}}{||x_{20}*x_{21}||}
$$
![](C:\Users\Administrator\Desktop\笔记图片\图形学\0001.JPG)

`Barycentric Interpolation`。一种插值方法，利用p点与三角形三条边张成的面积插值。具体公式如下所示，注意三个系数并不一定在[0,1]之间。
$$
A_0=\frac{1}{2}(x_1-p)\times(x_2-p)\cdot n \\
A_1=\frac{1}{2}(x_0-p)\times(x_2-p)\cdot n \\
A_2=\frac{1}{2}(x_0-p)\times(x_1-p)\cdot n \\ bi=\frac{A_i}{A},\sum bi=1,p = \sum b_i*x_i
$$
`Tetrahedral Volume`。求三棱锥体积，非常简单。

![](C:\Users\Administrator\Desktop\笔记图片\图形学\0002.JPG)
$$
V = \frac{1}{6}X_{30}*X_{10}\times X_{20}
$$
==Matrix==。矩阵的定义和简单运算。

transpose means 转置，diagonal means 对角，identity means 单位矩阵。

An orthogonal（正交的） matrix is a matrix made of orthogonal unit vectors.单位化的正交矩阵，不知道orthogonal这个包不包含乘积对角线上是unit元素的限制。$A^{T}A=I$就是它的定义。一个简要的性质就是$A^{-1}=A^{T}$。

==矩阵分解==。rotation+scaling。

`Singular Value Decomposition`

A matrix can be decomposed into:$A=UDV^{T}$。其中uv是正交矩阵，而D是奇异值矩阵。任何线性的形变都可以用三个步骤组成，旋转+缩放+旋转。

`Eigenvalue Decomposition`。一般只研究实对称阵的特征值分解。
$$
A\xi = \lambda\xi\ \ in\ \ some \ \ text \ \ books
\\or\ \ we \ \ can\ \ have \  \ A=UDU^{-1},U \ is\ \ orthogonal,A \ \ is \ \ symmetric
$$
`正定与半正定矩阵`。

![](C:\Users\Administrator\Desktop\笔记图片\图形学\0003.JPG)

对于正定矩阵，它的充要条件是特征值全正。一种充分不必要条件是对角占优。
$$
𝑎_{ii}>\sum_{(𝑖≠𝑗)}{|𝑎_{𝑖𝑗} |}  \ for\ \  all\ \  𝑖
$$
最后，正定的矩阵一定可逆。

==Linear Solver==。线性方程组求解，有多种方法，诸如LU分解法、牛顿方法、迭代法等。

`LU分解`。将原有的系数矩阵分解成下三角阵加上三角阵，这样消元将变得容易。

![](C:\Users\Administrator\Desktop\笔记图片\图形学\0004.JPG)

`迭代法`。简单来说可以使用以下的迭代公式。
$$
x_{k+1}=x_{k}+\alpha M^{-1}(b-Ax_{k})
$$
其中的alpha是relaxation factor属于自行调整的参数，括号里的那一项是残差，不同迭代法M矩阵选择不同。一些细节略去。

==Tensor Calculus==。`导数与梯度`。梯度的算符是一个倒三角形。稍微说明一下在矩阵表示下的求导运算，就是说函数f(X)中的X表示的是一个列向量，可以看成多个变元，那么对f(X)求偏导，结果是对x,y,z逐个求偏导，结果组成一个行向量。

![](C:\Users\Administrator\Desktop\笔记图片\图形学\0005.JPG)

`三维函数的旋度与散度`。它使用了不同于以往的记号，也就是说X是一个向量，可以把它理解成多元函数里的（x,y,z），这仅仅是记号的区别。

![](C:\Users\Administrator\Desktop\笔记图片\图形学\0006.JPG)

然后是二阶导。在泰勒展开这种近似当中，一般就只计算到二阶，更加高阶的项计算起来会很复杂。

![](C:\Users\Administrator\Desktop\笔记图片\图形学\0007.JPG)

### rigid body dynamics

刚体模拟可以归结为状态的更新过程。需要考虑的量会有时间、速度、位置和力，需要使用的技术主要是数值积分。那么一般可以用中间值近似，因为这是二阶准确的。

![](C:\Users\Administrator\Desktop\笔记图片\图形学\0008.JPG)

我们需要不断更新速度与位置的值。具体更新的时候有一些细节，比如说更新的顺序和使用的计算值。
$$
v^{[1]}=v^{[0]}+\Delta tM^{-1}f[0]\\
x^{[1]}=x^{[0]}+\Delta tv^{[1]}
$$


==旋转的表示==。旋转矩阵和欧拉角。

`欧拉角`。欧拉角是三个旋转自由度，它们有比较奇怪的名字，分别是roll、pitch、yaw三个，它们在三个坐标轴上作旋转，但是，这种方法的好处在于，一个复杂的旋转可以分解成为简单旋转的组合，它们被整理成一些标准方法，无需自己实现但是建议了解原理。

欧拉角的一个缺陷就是万向节死锁的存在和微分的困难。所谓万向节死锁（Gimbal Lock）是指在某些特殊位置下自由度的丢失。

`四元数`。关于四元数其实可以另开一章来专门讲述，它可以理解为一种四维空间向三维空间的投影方式。这里简单介绍它的定义、运算和怎么使用它来表示旋转。
$$
\begin{align}
&q=[s \ v],\ aq=[as\ av]\\&q_1+q_2 = [s_1+s_2,v_1+v_2],decrement \ is \ the \ same\\&q_1\times q_2 = [s_1s_2-v_1*v_2,s_1v_2+s_2v_1+v_1\times v_2]\\&||q||=\sqrt{s^2+v*v}\end{align}
$$
简单说明一下，四元数实际上可以视为复数的一种推广，v是一个复数矢量，也就是说，$v=xi+yj+zk$，其中i，j，k的平方均为-1，且ij = -ji = k。它们是两两正交的单位复向量。所以比较复杂一点的地方在于乘法，在乘法公式当中使用了普通向量的运算模式，这是可以证明的，可以直接使用，证明在此略去。

使用四元数可以表示以向量v为轴的theta角旋转，它的表示形式相当简洁。并且可以和转化为矩阵表示。

![](C:\Users\Administrator\Desktop\笔记图片\图形学\0009.JPG)

`刚体的平移与旋转`。

### rigid body contact

### clothes simulation

`Mass String`

其实感觉pbd的思路也很简单，王华民老师说的strain limiting，我感觉就是这个道理，首先使用其他的模型，比如普通受力分析、有限元方法等等，然后使用pbd做一个修正，使用限制（constrain）函数来更新点坐标和速度，实现起来是比较方便，但是没有物理意义，不太适合高精度仿真。



## Explict Integration&&Implict Integration

显式和隐式的欧拉积分。这是最为基础的模拟手段。显式积分比较简单，不再赘述。主要完成隐式积分的实现。

隐式积分的实现如下
$$
v^{[1]}=v^{[0]}+\Delta tM^{-1}f[1]\\x^{[1]}=x^{[0]}+\Delta tv^{[1]}
$$
从中得到
$$
v^{[1]}=(x^{[1]}-x^{[0]})/\Delta t
\\x^{[1]}=x^{[0]}+\Delta tv^{[0]}+\Delta t^2M^{-1}f^{[1]}
$$
然后，求解线性方程组转化为求一个优化问题
$$
x^{[1]} = arg\ min\ F(x)\\F(x)=\frac{1}{\Delta t^2}||x-x^{[0]}-\Delta tv^{[0]}||_M^2+E(x)
$$
求优化问题又可以采用Newton_Rapthon方法，迭代求解，原理如下

![](C:\Users\Administrator\Desktop\笔记图片\图形学\0010.JPG)

推导结果如下，相当于求导函数的根

![](C:\Users\Administrator\Desktop\笔记图片\图形学\0011.JPG)



## Projective Dynamics

前面已经学习了简单的PBD约束方式，可能还需要一点数学推导。

按照我的理解，PD是在隐式积分的基础上，对优化问题求解的改进。

`Frobenius Norm`。指的是矩阵所有元素的平方和开根号。
$$
||A||_F=\sqrt {tr(A^TA)}=\sqrt{\sum_{i,j}(x_{ij}^2)}
$$

矩阵导数相关。其实非常简单，矩阵变量的标量函数求导，就是标量函数值对每一个矩阵元素的求导。举个例子。
$$
\bigtriangledown||A||_F=\frac{A}{||A||_F}\\||A||_F=\sqrt{\sum_{i,j}(x_{ij}^2)},\frac{\part \sqrt{\sum_{i,j}(x_{ij}^2)}}{\part x_{ab}}=\frac{x_{ab}}{\sqrt{\sum_{i,j}(x_{ij}^2)}}
$$
看见了吧，就这么简单。那么，在隐式积分等价成优化问题的时候，还要用到另一个式子。抽象出来如下。其中B是一个m,m的对角阵，不妨设为$diag(\sqrt{\lambda_1},...\sqrt{\lambda_m})$，A是一个m,n的矩阵。
$$
\bigtriangledown_A||BA||_F=\frac{B^2A}{||BA||_F}\\
||BA||_F=\sqrt{\sum \lambda_i x_{i,j}^2},\frac{\partial \sqrt{\sum \lambda_i x_{i,j}^2}}{\partial x_{i,j}}=\frac{\lambda_ix_{i,j}}{ \sqrt{\sum \lambda_i x_{i,j}^2}}=\frac{B^2A}{||BA||_F}
$$
总而言之，只要明确了求梯度的定义，就可以方便地对向量、矩阵变元的**标量函数**求导，至于函数值也是矩阵的，暂时没有遇到。

















[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
