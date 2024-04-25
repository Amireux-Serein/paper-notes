# 前向过程

![](https://github.com/RuiqingTang/picx-images-hosting/raw/master/image/前向.45hbbbx3s.webp)

不断向上一时刻的数据中添加高斯噪声，最后变成几乎纯噪声。

该过程可以看作打标签的过程，因为噪声是人为构造的。



$$
{\alpha _{\rm{t}}} = 1 - {\beta _t}, ~ \beta要越来越大 \tag{1}
$$


$$
{x_t} = \sqrt {{\alpha _t}} {x_{t - 1}} + \sqrt {1 - {\alpha _t}} {z_1} \tag{2}
$$





从上一时刻加噪声得到当前时刻的数据，由于${\beta}$越来越大，所以$\alpha$越来越小。

越往后加的噪声越多，$ \sqrt {{\alpha_t}}$和$\sqrt {1 - {\alpha _t}}$可以看成是权重。


$$
{x_{t - 1}} = \sqrt {\alpha _{t - 1}} x_{t - 2} + \sqrt {1 - \alpha _{{t - 1}}} z_2 \tag{3}
$$


现在给定$x_0$，如何得到$x_t$？

将(3)代入到(2)，得：
$$
\begin{align}
x_t &= \sqrt {\alpha_t} (\sqrt {\alpha _{t - 1}} x_{t - 2} + \sqrt {1 - \alpha _{t-1}} z_2) + \sqrt {1 - \alpha_t} z_1 \\
    &= \sqrt {\alpha_t \alpha _{t - 1}} x_{t-2} + (\sqrt {\alpha_t (1 - \alpha _{t - 1})} z_2 + \sqrt {1 - \alpha_t} z_1) \tag{4} \\
    &= \sqrt {\alpha_t \alpha _{t - 1}} x_{t - 2} + \sqrt {1 - \alpha_t \alpha _{t - 1}} {z_2} 
\end{align}
$$
其中$z_1$，$z_2$均服从高斯分布（标准正态分布）

$z_1, z_2,... \sim N(0, ~ I)$

$\sqrt {1 - \alpha _t} z_1 \sim N(0, ~ 1 - \alpha_t)$​

$\sqrt {\alpha_t (1 - \alpha _{t - 1})} z_2 ~ \sim N(0, ~ \alpha_t (1 - \alpha _{t - 1}))$

不停迭代，最终得到：
$$
x_t = \sqrt {{\overline {\alpha}}_t} x_0 + \sqrt {1 - {\overline {\alpha}}_t}{z_t} \tag{6}
$$


任意时刻的数据，都可以通过初始的$x_0$得到。

# 逆向过程

![](https://github.com/RuiqingTang/picx-images-hosting/raw/master/image/逆向.8kzvuysbhd.webp)

已知$x_t$求$x_0$，很难一步求取。

迭代，由$x_1$求$x_{t - 1}$，由$x_{t - 1}$求$x_{t - 2}$，......，由$x_1$求$x_0$

$P(x_{t - 1} | x_t)$怎么求？

在前向过程中求得：$P(x_t | x_{t - 1})$，$P(x_t | x_0)$

贝叶斯公式：
$$
P(x_{t - 1} | x_t) = P(x_t | x_{t - 1}) \frac {P(x_{t - 1})} {P(x_t)} \tag{7}
$$

$$
P(x_{t - 1} | x_t, x_0) = P(x_t | x_{t - 1}, x_0) \frac {P(x_{t - 1} | x_0)} {P(x_t | x_0)} \tag{8}
$$

其中：
$$
P(x_t | x_{t - 1}, x_0) = \sqrt {\alpha_t} x_{t - 1} + \sqrt {1 - \alpha_t} z ~ \sim N(\sqrt {\alpha_t} x_{t - 1}, ~ 1 - \alpha_t) \tag{9}
$$

$$
P(x_{t - 1} | x_0) = \sqrt {{\overline {\alpha}} _{t - 1}} x_0 + \sqrt {1 - {\overline {\alpha}} _{t - 1}} z ~ \sim N(\sqrt {\overline {\alpha} _{t - 1}} x_0, ~ 1 - \overline {\alpha} _{t - 1}) \tag{10}
$$

$$
P(x_t | x_0) = \sqrt {{\overline {\alpha}}_t} x_0 + \sqrt {1 - {\overline {\alpha}}_t} z ~ \sim N(\sqrt {\overline {\alpha}_t} x_0, ~ 1 - \overline {\alpha}_t) \tag{11}
$$

(9)，(10)，(11)在前向过程中都能计算出来。

将标准正态分布展开：
$$
\begin{align}
N(\mu, ~ \sigma^2) &\propto e^ {-\frac {1} {2} \frac {(x - \mu)^2} {\sigma^2}} \tag{12} \\
&= e^ {- \frac {1} {2} (\frac {1} {\sigma^2}x^2 - \frac {2 \mu} {\sigma^2} x + \frac {\mu^2} {\sigma^2})}
\end{align}
$$
因此：
$$
\begin{align}
P(x_{t - 1} | x_t, x_0) & \propto e^ {- \frac {1} {2} (\frac {(x_t - \sqrt {\alpha_t} x_{t - 1})^2} {\beta_t} + \frac {(x_{t - 1} - \sqrt {\overline {\alpha} _{t - 1}} x_0)^2} {1 - \overline {\alpha} _{t - 1}} - \frac {(x_t - \sqrt {\overline {\alpha}_t} x_0)^2} {1 - \overline {\alpha}_t})} \\
&= e^ {- \frac {1} {2} (\frac {x^2_t - 2 \sqrt {\alpha_t} x_t x_{t - 1} + \alpha_t x^2_{t - 1}} {\beta_t} + \frac {x^2_{t - 1} - 2 \sqrt {\overline {\alpha} _{t - 1}} x_0 x_{t - 1} + \overline {\alpha} _{t - 1} x^2_0} {1 - \overline {\alpha} _{t - 1}} - \frac {(x_t - \sqrt {\overline {\alpha}_t} x_0)^2} {1 - \overline {\alpha}_t})} \tag{13} \\
&= e^ {- \frac {1} {2} ((\frac {\alpha_t} {\beta_t} + \frac {1} {1 - \overline {\alpha} _{t - 1}}) x^2_{t - 1} - (\frac {2 \sqrt {\alpha_t}} {\beta_t} x_t + \frac {2 \sqrt {\overline {\alpha} _{t - 1}}} {1 - \overline {\alpha}_{t-1}} x_0) x_{t - 1} + C(x_t, ~ x_0))}
\end{align}
$$
计算$\mu$可以得到：
$$
\widetilde {\mu}_t (x_t, ~ x_0) = \frac {\sqrt {\alpha_t} (1 - \overline {\alpha} _{t - 1})} {1- \overline {\alpha}_t} x_t + \frac {\sqrt {\overline {\alpha} _{t - 1}} \beta_t} {1 - \overline {\alpha}_t} x_0 \tag{14}
$$
$x_0$由(6)逆推得到表达式：
$$
x_0 = \frac {1} {\sqrt {\overline {\alpha}_t}} (x_t - \sqrt {1 - \overline {\alpha}_t} z_t) \tag{15}
$$
(15)代入(14)：
$$
\widetilde {\mu}_t = \frac {1} {\sqrt {\alpha_t}} (x_t - \frac {\beta_t} {\sqrt {1 - \overline {\alpha}_t}} z_t) \tag{16}
$$
$z_t$​​如何处理？

无法直接求得解析解，使用神经网络来求取近似解。

Unet架构，输入参数为当前时刻的分布、时刻t。

![](https://github.com/RuiqingTang/picx-images-hosting/raw/master/image/algo.231o1nh6cg.webp)







