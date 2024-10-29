# Lecture 7: Interest Point Detection

> 将关注图片 Sequence

## Feature Detection

Feature: 一张图片 interesting 的部分

Local Feature:

- Locality 
- Generalisable
- Abundant
- Efficiency

Typr of Features

- Edge
- Corners
- Blobs
- Ridges

## Harris Detector

如果我们将
$$
\varepsilon(u,v) = \sum_{x, y} \left[ \underbrace{I(x+u, y+v)}_\text{shifted intensity} - \underbrace{I(x, y)}_\text{intensity} \right]^2
$$
如使用 Taylor Series 进行展开，则有
$$
\begin{align}
I(x+u, y+v) 
&= I(x, y) + \frac{\part I}{\part x}u + \frac{\part I}{\part y}v + \cdots
\\ &\approx I(x, y) + \frac{\part I}{\part x}u + \frac{\part I}{\part y}v
\\
\varepsilon(u,v)
&= \sum_{x, y} \left[ I(x+u, y+v) - I(x, y) \right]^2
\\&\approx  \sum_{x, y} \left[ I(x, y) + \frac{\part I}{\part x}u + \frac{\part I}{\part y}v - I(x, y)) \right]^2
\\&=  \sum_{x, y} \left[ \frac{\part I}{\part x}u + \frac{\part I}{\part y}v \right]^2
\end{align}
$$
如果定义 $I_x = \frac{\part I}{\part x}, I_y = \frac{\part I}{\part y}$，则有：
$$
\begin{align}
\varepsilon(u,v)
  &= \sum_{x, y} \left[ I(x+u, y+v) - I(x, y) \right]^2
\\&= \sum_{x, y} \left[ \frac{\part I}{\part x}u + \frac{\part I}{\part y}v \right]^2
\\&= \sum_{x, y} \left[ I_xu +I_yv \right]^2
\\&= \sum_{x, y} \left[\left[ u, v \right]\left[ I_x, I_y \right]^T\right]^2
\\&= \sum_{x, y} \left[\left[ u, v \right]\left[ I_x, I_y \right]^T\left[ I_x, I_y \right]\left[ u, v \right]^T\right]
\end{align}
$$


> **特征值**
>
> 对于一个方阵 $A$，如非 0 向量满足：
> $$
> A x = \lambda x, \quad \text{where } x, \lambda \neq 0
> $$
> 那么向量 $x$ 为矩阵 $A$ 的一个特征向量，$\lambda$​ 则为其对应的特征值
>
> 左侧，可以看作 $x$ 经过 $A$ 变换投影到 $A$ 的基坐标中
> 右侧，可以将 $\lambda$ 看作在对 $x$​ 进行伸缩变换
>
> 因此特征向量是投影到方阵定义的空间只发生伸缩变化，而不发生旋转变化的向量。
>
> 如果一个 $n$ 阶方阵 $A$ 所有特征值不相等，那么该向量有 $n$ 个相互独立（正交）的特征向量。

> 对角化
>
> 如果存在可逆矩阵 $P$ 使得 $P^{-1} AP$ 为对角矩阵，则称 $A$ 为可对角化的：
> $$
> P^{-1}AP=\Lambda
> $$
>
> > 对角矩阵 Diagonal Matrix
> > $$
> > \begin{bmatrix}
> > a_1 & \\
> >     & a_2\\
> >     &     & \ddots \\
> >     &     &        & a_n
> > \end{bmatrix}
> > $$
> > 
>
> $n$ 阶方阵 $A$ 有 $n$ 个互异特征值，则 $A$ 必可对角化。
>
> 充分性：
> $$
> Ax = \lambda x\\
> \text{if } x = P, \lambda=\Lambda \text{, then } AP = \Lambda P, P^{-1} AP =\Lambda
> $$
> 必要性：
> $$
> P^{-1}AP = \Lambda\\AP=\Lambda P
> $$
> 如考虑 $P$ 为特征向量构成的方阵，$\Lambda$ 为特征值构成的对角矩阵。则称呼这个过程为矩阵的特征分解。
>
> 而考虑上述情况
>
> 
>
> 当我们构造特征向量矩阵 P 时,我们通常会选择**标准正交的特征向量组**作为其列向量。也就是说:
>
> - 所有特征向量都被归一化为单位向量
> - 不同特征向量两两正交
>
> ，也拥有性质
> $$
> P^TP = I, P^{-1}=P^T
> $$
