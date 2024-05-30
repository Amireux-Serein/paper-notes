![](https://github.com/RuiqingTang/picx-images-hosting/raw/master/image/image.1sevnq20d1.webp)

<div style="text-align: center;font-size: 20px;">Figure 1. Deformable 3DGS</div>

![](https://github.com/RuiqingTang/picx-images-hosting/raw/master/image/image.7ljtx0q6cf.webp)

<div style="text-align: center;font-size: 20px;">Figure 2. Original 3DGS</div>

大部分的内容都很相似，只不过Deformable 3DGS引入了一个MLP，除此之外，它还增加了一个退火平滑训练机制AST（annealing smoothing training mechanism）。

简要讲解：同原版的3DGS一样，从初始化的SfM点云开始，以每个点为中心生成3D Gaussian。将3D Gaussian的位置 $\boldsymbol {x}$ 和时间 $t$ 进行位置编码，然后输入到MLP网络，得到Gaussians在canonical space的偏移量 $(\delta _{\boldsymbol {x}} , \delta _{\boldsymbol {r}} , \delta _{\boldsymbol {s}})$ ，然后加上原本的Gaussians，得到变形Gaussians。之后的步骤和原版3DGS一样，只是多了一个AST和梯度也要回传到MLP来更新参数。

以下是个人的一些理解：

变形场是通过MLP来实现的，它的作用是将Gaussian映射到canonical space。MLP的输入是经过位置编码的Gaussian的位置和时间，输出是位置，旋转，缩放的偏移量。

位置编码的实现：


$$
\gamma(p) = \left(\sin(2^k \pi p),\cos(2^k \pi p) \right) _{k=0} ^{L-1}
$$


合成的场景数据：对于位置 $\boldsymbol{x}$ ， $L=10$ ；对于时间 $t$ ， $L=6$ 

真实的场景数据：对于位置 $\boldsymbol{x}$ ， $L=10$ ；对于时间 $t$ ， $L=10$ 

这个过程可以表示为： 


$$
( \delta _{\boldsymbol {x}}, \delta _{\boldsymbol {r}}, \delta _{\boldsymbol {s}})  = \mathcal {F} _{\theta}(\gamma(sg(\boldsymbol {x})), \gamma(t))
$$


其中， $\mathcal {F} _{\theta}$ 表示MLP。 $sg(·)$ 表示停止梯度操作。

简单来说就是变形场的功能就是：根据时间输入给每个Gaussian提供相应的相对应的变形参数，使得其在canonical space里保持一致或稳定。通过学习时间和位置的关系，变形场就能够捕捉到场景中的动态变化。

AST

没太看懂......


$$
\boldsymbol {\Delta} = \boldsymbol {\mathcal {F}} _{\boldsymbol {\theta}} (\boldsymbol {\gamma} (sg(\boldsymbol {x})), \boldsymbol {\gamma} (t) + \boldsymbol {\mathcal {X}} (i))
$$


$$
\boldsymbol {\mathcal {X}} (i) = \mathbb {N} (0, 1) · \beta · \Delta t · (1 - i/ \tau)
$$


