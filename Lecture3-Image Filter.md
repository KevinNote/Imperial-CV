# Lecture 3: Image Filter

## Foundamental Filter

如另 $\Omega_{i,j}$ 为对于位置 $(i, j)$ 的 Neighbour，则可以求其信号中位数 $Median_{(m, n) \in \Omega_{ij}} f(m,n)$ 和 均值 $Mean_{(m, n) \in \Omega_{ij}} f(m,n)$

可有noise cancelling 算法：

**Mean Filter**
$$
g(i, j) = Mean_{(m, n) \in \Omega_{ij}} f(m,n)
$$

**Median Filter**
$$
g(i, j) = Median_{(m, n) \in \Omega_{ij}} f(m,n)
$$

## Convolution

![](img/lec3/signal.jpeg)

对于 1D 信号，有
$$
\underbrace{f'(x)}_{\text{New Signal}} = \int_{-\infty}^{\infty}\underbrace{f(x)}_{\text{Original Signal}}\underbrace{\kappa(x-\tau)}_{\text{Kernel}} d\tau
$$
即 Kernel 会被**左右翻转**。

如果演进到 2D 信号，则：
$$
\underbrace{f'(x_1, x_2)}_{\text{New Signal}} = \int_{-\infty}^{\infty}\underbrace{f(x_1, x_2)}_{\text{Original Signal}}\underbrace{\kappa(x_1-\tau_1, x_2-\tau_2)}_{\text{Kernel}} d\tau_1 d \tau_2
$$
即 Kernel 会被**左右翻转**，**上下翻转**。



