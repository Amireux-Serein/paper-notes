# 球谐函数

有点类似傅里叶级数的思想，先看傅里叶级数:



$$
f(x) = a_0 + \sum _{n=1} ^{\infty} a_n cos{\frac {n \pi} {l} x} + b_n sin{\frac {n \pi} {l} x}
$$

 

其中： $cos{\frac {n \pi} {l} x}$ 和 $sin{\frac {n \pi} {l} x}$ 叫做基函数，当 $n \rightarrow + \infty$ 时，就几乎完美拟合任意曲线了。当 $n$ 有限时，也可以近似。

球谐函数也是类似的，用基函数来拟合。


$$
\begin{align}
f(\theta, \phi) &\approx a_0 f _{l=0, m=0} f(\theta, \phi) + a_1 f _{l=1, m=-1} \\ &+ a_2 f _{l=1, m=0} f(\theta, \phi) + a_3 f _{l=1, m=1} f(\theta, \phi) + ......
\end{align}
$$



![](https://github.com/RuiqingTang/picx-images-hosting/raw/master/image/sh_fun.9dcspjecbv.webp)

球谐函数在3DGS里用来表示点云的颜色 $(r,g,b)$​ ，这样可以在不同的角度呈现不同的颜色。

# 3D Gaussian

$n$​ 维高斯


$$
f(\mathbf{x}; \mathbf{\mu}, \Sigma) = \frac{1}{\sqrt{(2\pi)^n } |\Sigma| ^{\frac {1} {2}}} \exp\left(-\frac{1}{2} (\mathbf{x} - \boldsymbol{\mu})^\top \Sigma^{-1} (\mathbf{x} - \boldsymbol{\mu})\right) \tag{1}
$$


先看一维的情况，标准的高斯分布，均值为0，方差为1：


$$
f(x) = \frac {1} {\sqrt {2 \pi}} \exp \left(- \frac {x^2} {2} \right) \tag{2}
$$


一般形式，均值为 $\mu$ ，方差为 $\sigma$​ ：


$$
f(x) = \frac {1} {\sqrt {2 \pi} \sigma} \exp \left(- \frac {(x - \mu) ^2} {2 \sigma ^2} \right) \tag{3}
$$


相比于标准高斯分布，向右平移了 $\mu$ 个单位，函数的宽度延展了 $\sigma$ 倍。为了保证概率密度函数的积分为1， $f(x)$ 的高度就会下降，表现在系数的分母部分除以 $\sigma$ 。

扩展到n维，假设有n个服从高斯分布且互不相关的独立变量 ：


$$
\mathbf{x} = [x_1,x_2,...,x_n] ^\top \tag{4}
$$


均值为：


$$
\mathbf {E} (\mathbf {x}) = [u_1,u_2,...,u_n] ^\top \tag{5}
$$


方差为：


$$
\sigma (\mathbf {x}) = [\sigma _1, \sigma _2,..., \sigma _n] ^\top \tag{6}
$$


因此概率密度公式为：


$$
\begin{align}
f(\mathbf {x}) &= p(x_1,x_2,...,x_n) \\
&= p(x_1)p(x_2)...p(x_n) \\ 
&= \frac {1} {(\sqrt {2 \pi}) ^n \sigma _1 \sigma _2 \cdots \sigma _n} \exp \left(-\frac {(x_1 - \mu _1) ^2} {2 \sigma _1^2} -\frac {(x_2 - \mu _2) ^2} {2 \sigma _2^2} - \cdots -\frac {(x_n - \mu _n) ^2} {2 \sigma _n^2} \right) 
\end{align} \tag{7}
$$


设：


$$
\mathbf {z} ^2 = \frac {(x_1 - \mu _1) ^2} {\sigma _1^2} + \frac {(x_2 - \mu _2) ^2} {\sigma _2^2} + \cdots + \frac {(x_n - \mu _n) ^2} {\sigma _n^2} \tag{8}
$$

$$
\sigma _{\mathbf {z}} = \sigma _1 \sigma _2 \cdots \sigma _n \tag{9}
$$



则可以将 (7) 重写为：


$$
f(\mathbf {z}) = \frac {1} {(\sqrt {2 \pi}) ^n \sigma _{\mathbf {z}}} \exp \left(-\frac {\mathbf {z} ^2} {2} \right) \tag{10}
$$


其中，


$$
\begin{align}
\mathbf {z} ^2 &= \mathbf {z} ^\top \mathbf{z} \\
&= [x_1 - \mu _1,x_2 - \mu _2, \cdots, x_n - \mu _n] 
\begin{bmatrix}
\frac {1} {\sigma _1^2} & 0 & \cdots & 0 \\
0 & \frac {1} {\sigma _2^2} & \cdots & 0 \\
\vdots & \cdots & \cdots & \vdots \\
0 & 0 & \cdots & \frac {1} {\sigma _n^2}
\end{bmatrix}
[x_1 - \mu _1,x_2 - \mu _2, \cdots, x_n - \mu _n] ^\top \tag{11}
\end{align}
$$



设：


$$
[\mathbf {x} - \boldsymbol {\mu}] = [x_1-\mu _1,x_2 - \mu _2,...,x_n - \mu _n] ^\top \tag{12}
$$


于是有：


$$
\begin{align}
\mathbf {z} ^2 
= [\mathbf {x} - \boldsymbol {\mu}] ^\top
\begin{bmatrix}
\frac {1} {\sigma _1^2} & 0 & \cdots & 0 \\
0 & \frac {1} {\sigma _2^2} & \cdots & 0 \\
\vdots & \cdots & \cdots & \vdots \\
0 & 0 & \cdots & \frac {1} {\sigma _n^2}
\end{bmatrix}
[\mathbf {x} - \boldsymbol {\mu}] \tag{13}
\end{align}
$$


协方差矩阵 $\Sigma$ ，为 $x_i$ 和 $x_j$ 的协方差，由于 $x_i$ 和 $x_j$​ 相互独立，所以只有对角线上存在元素。


$$
\Sigma=
\begin{bmatrix}
\sigma _1^2 & 0 & \cdots & 0 \\
0 & \sigma _2^2 & \cdots & 0 \\
\vdots & \cdots & \cdots & \vdots \\
0 & 0 & \cdots & \sigma _n^2 \tag{14}
\end{bmatrix}
$$


综上，式 (10) 可以重写为：


$$
f(\mathbf {z}) = \frac {1} {(\sqrt {2 \pi}) ^n |\Sigma| ^{\frac {1} {2}}} \exp \left(- \frac {(\mathbf {x} - \boldsymbol {\mu}) ^\top (\Sigma) ^{-1} (\mathbf {x} - \boldsymbol {\mu})} {2} \right) \tag{15}
$$


为什么会是椭球？

令 $n=3$ ，固定概率为 $p$​ ，对式 (15) 取对数得到：


$$
 -\ln (2 \pi) ^{\frac {3} {2}} |\Sigma| ^{\frac {1} {2}} - \frac {(\mathbf {x} - \boldsymbol {\mu}) ^\top (\Sigma) ^{-1} (\mathbf {x} - \boldsymbol {\mu})} {2} = \ln p \tag{16}
$$

$$
(\mathbf {x} - \boldsymbol {\mu}) ^\top (\Sigma) ^{-1} (\mathbf {x} - \boldsymbol {\mu}) = - 2 \ln (2 \pi) ^{\frac {3} {2}} |\Sigma| ^{\frac {1} {2}} - 2 \ln p = C \tag{17}
$$

$$
\frac {(x_1 - \mu _1) ^2} {\sigma _1^2} + \frac {(x_2 - \mu _2) ^2} {\sigma _2^2} + \frac {(x_3 - \mu _3)^2} {\sigma _3^2} = C \tag{18}
$$



论文里省略了系数，公式如下：


$$
G(x) = \exp \left(- \frac {1} {2} (x) ^\top \Sigma ^ {-1} (x) \right) \tag{19}
$$



# 基于点的渲染

公式如下：


$$
C = \sum _{i=1} ^N T_i (1 - \exp (- \sigma _i \delta _i)) c_i ， ~ T_i = \exp \left(- \sum _{j=1} ^{i-1} \sigma _j \delta _j \right) \tag{20}
$$


密度 $\delta$ ，透光率 $T$ ，颜色 $c$ 沿光线间隔 $\delta _i$​ 采样得到。

定义：


$$
\alpha _i = (1 - \exp (-\sigma _i \delta _i)), ~ T_i = \prod _{j=1} ^{i-1} (1 - \alpha _i) \tag{21}
$$


将(21)中的两个公式代入(20)，可以得到：


$$
C = \sum _{i=1} ^{N} T_i \alpha _i c_i \tag{22}
$$



在3DGS中，将3d gaussians (ellipsoids) 投影到二维的空间平面上进行渲染。

给定世界到相机的变换矩阵 $W$ ，相机到屏幕空间的映射矩阵 $P$ （论文里作者使用 $J$ 来表示 $P$ 的雅可比矩阵），由空间协方差矩阵 $\Sigma$ 投影到平面上的协方差矩阵 $\Sigma'$​ 由以下公式计算得到：


$$
\Sigma' = J W \Sigma W ^\top J ^\top \tag{23}
$$


由于 $\Sigma$ 在半正定的时候才具有物理意义，现在如果直接对 $\Sigma$ 进行优化，那么使用梯度下降就无法保证这一条件。因此作者使用了一种等价表示，给定缩放矩阵 $S$ 和旋转矩阵 $R$​ ，协方差矩阵可以表示为：


$$
\Sigma = R S S ^\top R ^\top \tag{24}
$$


三维向量和四元数分别表示 $S$ 和 $R$ 。显式推导各个参数的梯度，避免自动微分的计算开销。

# 关于splatting的具体实现

![](https://github.com/RuiqingTang/picx-images-hosting/raw/master/image/image.13lm1cbrre.webp)

给定一个像素点 $x$ 的位置，通过变换矩阵 $W$ 可以计算出像素点到所有重叠的重叠的高斯点的距离，就是高斯点的深度，以深度进行排序可以得到长度为 $N$ 的列表（点云中一定半径范围内能影响像素的 $N$ 个有序点来计算一个像素的颜色 $C$​ ）。


$$
C = \sum _{i \in N} c_i \alpha _i ' \prod _{j=1} ^{i-1} (1 - \alpha _j') \tag{25}
$$


其中， $c_i$ 是当前学到的颜色， $\alpha _i '$ 是当前学到的不透明度， $\prod _{j=1} ^{i-1} (1 - \alpha _j')$ 作为权重， 如果前面的点越透明，那么这个权重就会越大。或者也可以理解成最终学到的结果，拿去做渲染。

将Gaussian乘上不透明度：


$$
\alpha _i' = \alpha _i \times \exp \left(- \frac {1} {2} (\boldsymbol {x}' - \boldsymbol {\mu} _i') ^\top \Sigma _i '^{-1} (\boldsymbol {x} - \boldsymbol {\mu} _i') \right) \tag{26}
$$


保留99%置信区间的Gaussian：将Gaussian投影到平面上，得到了一个新的Gaussian，针对某个像素来说，如果它有99%的可能性落在这个Gaussian里，那么就保留这个Gaussian。

3DGS采用的是tile_based的方案，将图像划分成16*16的tiles（原始论文的说法）

将图像划分成若干个互不重叠的tiles，每个tiles里是16*16个像素（综述的说法）

这两个有着本质区别，需要查看源码才知道具体实现是什么样的。剩下的步骤都是一样的。

排序：可以简单理解为给每个tile分配一个id，根据Gaussian的深度进行排序。实际要复杂得多，会给每个Gaussian分配一个键，这个键结合了深度和id，使用了一个叫GPU Radix排序的方法，根据这个键排序。

每个tile分配一个线程块，对于其中的每个像素（遍历从影响tile中所有像素的最后一个点开始），每个像素从深度小于或等于最后一个对颜色做贡献的Gaussian开始。对颜色和不透明度进行累加，一个像素的不透明度达到1的时候，对应的线程就会终止，当tile中所有像素的不透明度都达到1的时候，整个tile的线程块就终止了。

# 密度控制

![](https://github.com/RuiqingTang/picx-images-hosting/raw/master/image/image.45hi3jjaq8.webp)

首先，每100个 iter ，会进行一次，增加一些Gaussian。

对于不透明度非常低的， $\alpha$​​ 低于某个阈值，直接删除。

对于相机附近（离相机距离非常近）出现artifacts，或者说floaters，一般是不合理的增加Gaussian导致，每3000 iter 将Gaussian的 $\alpha$ 设置为0，后续的优化中会将必要的Gaussian的 $\alpha$ 增大。周期性地删除不合理的Gaussian。

剩下两种情况，通常是梯度较大的区域，论文的阈值设置为0.0002。

对于重建不足的区域，克隆一个Gaussian，然后沿着梯度方向移动。

对于过度重建的区域，将其分裂成两个，并把scale系数除以超参数1.6，位置由PDF采样来初始化。













