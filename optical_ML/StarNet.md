# StarNet论文笔记

20250218

# information

**paper**: Rewrite the Stars

**report/doi**: https://arxiv.org/abs/2403.19967

**code**: https://github.com/ma-xu/rewrite-the-stars

**demo**: None

**level**: CVPR2024

# 论文内容

研究神经网络中的“星操作”，即逐元素乘法。

## 效果

将输入映射到极高维的非线性特征空间。

## 原理

星操作以下简记为sop

单层网络中：
$$
sop = (W_1^TX+B_1)*(W_2^TX+B_2) \tag1
$$
逐元素乘法融合两个线性变换的特征。

将权重矩阵和偏置合并，记作 $W=[W,B]^T$ ，同理 $X=[X,1]^T$ ，从而有：
$$
sop = (W_1^TX)*(W_2^TX) \tag2
$$
分析单元素输入和单输出通道变换的情况：

定义 $w_1,w_2,x \in \mathbb{R}^{(d+1)×1}$ ，其中 $d$ 是输入通道数。

如果拓展到多个输出通道，处理多个特征元素，那么有：
$$
W_1, W_2 \in \mathbb{R}^{(d+1)×(d^{'}+1)} \tag3
$$
其中，$X \in \mathbb{R}^{(d+1)×n}$ 。

作者将sop重写为：
$$
\begin{align}
\text{sop} &= w_{1}^{\mathrm{T}}x \star w_{2}^{\mathrm{T}}x \\
&= \left(\sum_{i=1}^{d+1}w_1^ix^i\right) \cdot \left(\sum_{j=1}^{d+1}w_2^jx^j\right) \\
&= \sum_{i=1}^{d+1}\sum_{j=1}^{d+1}w_1^iw_2^jx^ix^j \\
&= \underbrace{\alpha_{(1,1)}x^{1}x^{1}+\cdots+\alpha_{(4,5)}x^{4}x^{5}+\cdots+\alpha_{(d+1,d+1)}x^{d+1}x^{d+1}}_{\frac{(d+2)(d+1)}{2}\text{ items}} \tag4
\end{align}
$$
$i,j$ 是通道的索引，而$\alpha$ 是每项的系数。
$$
\left.\alpha_{(i,j)}=\left\{
\begin{array}
{cc}w_1^iw_2^j & \mathrm{~if~}i==j, \\
w_1^iw_2^j+w_1^jw_2^i & \mathrm{~if~}i!=j.
\end{array}\right.\right.
$$
$d$ 维空间计算，映射到 $\frac{(d+2)(d+1)}{2}$ 维特征空间（不准确，因为 $\alpha_{(d+1,:)}x^{d+1}x$ 是和 $x$ 线性关联的，但是应该可以忽略不记）。

类似的可以进行多层网络的推导，设网络为 $l$ 层，通道数为 $d$ ，最终获得的特征空间维度近似为 $\mathbb{R}^{(\frac {d} {\sqrt{2}})^{2^l}}$​ 。显著放大隐式维度。

# 启示

用来进行轻量化网络的搭建，作者提出的demo模型，可以在iPhone13上以0.7s的延迟进行预测，适合用来进行DAS领域的实时入侵检测，算力消耗少，精度较高。

尚未进行复现，作者训练的源码运行在集群上而非普通PC，需要重构。目前交给陆星宇作为毕设的基础网络结构，数据集使用北交开源数据集。