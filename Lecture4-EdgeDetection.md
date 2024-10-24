# Lecture 4: Edge Detection

[TOC]



## Characteristic of Edge

当出现边缘时候，Image Intensity Function（i.e. 信号值）会快速更改。

```
___     ___
   _____
   ^^^^^
   Edge
```

其在 Intensity 上的一阶导便会有起伏。

问题是我们怎么找到一阶导。

### Filter for 1st Derivative

#### Roberts Operator

$$
h_1=\begin{bmatrix}
1 & 0\\
0 & -1
\end{bmatrix}
\quad
h_2=\begin{bmatrix}
 0 & 1\\
-1 & 0
\end{bmatrix}
$$

#### Prewitt Operator

$$
h_1=\begin{bmatrix}
1 & 1 & 1\\
0 & 0 & 0\\
-1 & -1 & -1
\end{bmatrix}
\quad
h_2=\begin{bmatrix}
 0 & 1 & 1\\
-1 & 0 & 1\\
-1 & -1 & 0
\end{bmatrix}
\quad
h_3=\begin{bmatrix}
 -1 & 0 & 1\\
-1 & 0 & 1\\
-1 & 0 & 1
\end{bmatrix}
$$

#### Sobel Operator

$$
h_1=\begin{bmatrix}
1 & 2 & 1\\
0 & 0 & 0\\
-1 & -2 & -1
\end{bmatrix}
\quad
h_2=\begin{bmatrix}
 0 & 1 & 2\\
-1 & 0 & 1\\
-2 & -1 & 0
\end{bmatrix}
\quad
h_3=\begin{bmatrix}
 -1 & 0 & 1\\
-2 & 0 & 2\\
-1 & 0 & 1
\end{bmatrix}
$$

#### Robinson Operator

$$
h_1=\begin{bmatrix}
1 & 1 & 1\\
1 & -2 & 1\\
-1 & -1 & -1
\end{bmatrix}
\quad
h_2=\begin{bmatrix}
 1 & 1 & 1\\
-1 & -2 & 1\\
-1 & -1 & 1
\end{bmatrix}
\quad
h_3=\begin{bmatrix}
 -1 & 1 & 1\\
-1 & -2 & 1\\
-1 & 1 & 1
\end{bmatrix}
$$

#### Kirsch Operator

$$
h_1=\begin{bmatrix}
3 & 3 & 3\\
3 & 0 & 3\\
-5 & -5 & -5
\end{bmatrix}
\quad
h_2=\begin{bmatrix}
3 & 3 & 3\\
-5 & 0 & 3\\
-5 & -5 & 3
\end{bmatrix}
\quad
h_3=\begin{bmatrix}
-5 & 3 & 3\\
-5 & 0 & 3\\
-5 & 3 & 3
\end{bmatrix}
$$

### Magtitude

如果我们应用 filter $h$ 可得 $g = h*f$。

如考虑 $x$ 和 $y$ 轴分别拥有过滤器 $h_x, h_y$则可以获得 Gradient Magnitude（梯度幅度）：
$$
g(x) =\sqrt{g_x^2+g_y^2}
\\\text{where }
g_x = h_x* f , g_y=h_y * f
$$
Magnitude 越高越可能为边缘。

 ### Direction

考虑 $h_x$ 为 $x$ 轴的梯度，$h_y$ 为 $y$ 轴的梯度，我们可以获得梯度方向：
$$
\tan^{-1}\frac{g_y}{g_x}
$$

> $$
> \tan \theta = 对边/ 邻边
> $$

Edge Gradient 垂直于（perpendicular to）Edge Direction。

## Canny Filter

### Criteria

Canny 定义了如下 criteria：

- **Good Detection**
  应当具有较低的概率会遗漏真实边缘点
  同时也应具有较低的概率错误地将非边缘点标记为边缘。
- **Good Localization**
  算子标记的边缘点应当尽可能接近真实边缘的中心。
- **Single Response**
  对单个边缘只产生一个响应。

### Process

在最早我们使用如下流程去做边缘检测，其中 EstGrad 是应用之前的核

```mermaid
flowchart LR
	DeN[Denoise<br>去噪]
	EstGrad[Estimate Gradient<br>估计梯度]
	GlbThr[Global Threshold<br>全局阈值/二值化]
	
	DeN ---> EstGrad ---> GlbThr
```

Canny 提供了一个新的边缘检测算法：

```mermaid
flowchart LR
	DeN[Gaussian Filter<br>Suppress Noise<br>Gaussian 核去噪]
	EstGrad[Gradient Magnitude & <br> Direction<br>梯度幅度和方向]
	NMS[NMS for Single Response<br>应用 NMS 获得每个<br>边缘的单一响应]
	GlbThr[Hysteresis Thresholding<br>滞后阈值法查找潜在边缘]
	
	DeN ---> EstGrad ---> NMS ---> GlbThr
```



## 2nd Order Detection

> 参阅 https://www.bilibili.com/video/BV1168weEEwu

![](./img/Lec4/2ndDeri.png)

可以用二阶导过零点（Zero-crossing）定位边缘.

### Laplacian

拉普拉斯核是由 横向的二阶导 + 纵向的二阶导得来
$$
\begin{bmatrix}
0 & 1 & 0\\
0 & -2 & 0\\
0 & 1 & 0
\end{bmatrix} +
\begin{bmatrix}
0 & 0 & 0\\
1 & -2 & 1\\
0 & 0 & 0
\end{bmatrix}
=
\begin{bmatrix}
0 & 1 & 0\\
1 & -4 & 1\\
0 & 1 & 0
\end{bmatrix}
$$
另一种拉普拉斯核为
$$
\begin{bmatrix}
1 & 1 & 1\\
1 & -8 & 1\\
1 & 1 & 1
\end{bmatrix}
$$
其考虑更多邻居。

> **Mathematics behind Laplacian Kernal**
>
> 拉普拉斯算子的定义为：
> $$
> \Delta f = \nabla^2f = \nabla\cdot \nabla f
> $$
> 即二阶导。其再笛卡尔坐标系 $x_i$ 被定义为：
> $$
> \Delta f =\sum^n_{i=1}\frac{\part^2 f}{\part x_i^2}
> $$
> 即在二维情况下定义为
> $$
> \Delta f = \frac{\part^2f}{\part x^2} + \frac{\part^2 f}{\part y^2}
> $$
> 考虑图像是离散，因此需考虑其差分式：
>
> 一阶导的离散近似（Forward Difference）：
> $$
> f'(x)\approx \frac{f(x+h) - f(x)}{h}
> $$
> 其中 $h$ 为步长
>
> 二阶导为一阶导的导数，则有：
> $$
> \begin{align}
> f''(x)&=\frac{f'(x+b) - f'(x)}{h}\\
> &= \frac{\frac{f(x+2h)-f(x+h)}{h} - \frac{f(x+h)-f(x)}{h}}{h}\\
> &= \frac{f(x+2h)-f(x+h) - f(x+h)+f(x)}{h^2}\\
> &= \frac{f(x+2h)-2f(x+h)+f(x)}{h^2}\\
> \end{align}
> $$
> 令 $h=1$，则有：
> $$
> \begin{align}
> f''(x)
> &= f(x) - 2f(x+1) + f(x+2)\\
> &= \begin{bmatrix}1 \\ -2 \\1 \end{bmatrix} ^T
> \begin{bmatrix}f(x) \\ f(x+1) \\f(x+2)\end{bmatrix}
> 
> \end{align}
> $$

![](./img/Lec4/2ndDeri-demo.png)



### Marr-Hildreth Edge-Detection

$$
\text{Marr-Hildreth} = \text{Gaussian Filter} + \text{Laplacian}
$$

考虑 拉普拉斯对噪音非常敏感，因此使用高斯核先进行 smoothing，在使用 Laplacian 作为 criterion。

1. $n\times n$ 的高斯 Lowpass 核 进行 smoothing
2. 计算图片的 Laplacian
3. 寻找 Zero crossing
4. (extra) Thresholding ($V > T$)

对于高斯核，其参数 $\sigma$ 决定其受邻居影响有多大。越大的 $\sigma$ 越 smooth，也就意味着更强壮的边缘才能被检测。

### LoG: Laplacian of Gaussian

```mermaid
flowchart LR
img[Image]
simg[Smoothed<br>Image]
lap[Laplacian]
img --"*"--> simg
simg --"*"--> lap
```

考虑上述 MH 的流程，考虑卷积的 Associative，即：
$$
f * G * L = f * (G * L)
$$
其中 $G * L$  即为 Laplacian of Gaussian。

> **Mathematics behind LoG**
> $$
> \\
> \begin{align}
> G(x, y) &= e ^{-\frac{x^2+y^2}{2\sigma^2}}
> \\
> \nabla^2 G(x, y)&=
> \frac{\part^2 G(x, y)}{\part x^2} +
> \frac{\part^2 G(x, y)}{\part y^2}
> \\
> &=
> \frac{\part}{\part x} \left(\frac{-x}{\sigma^2}e^{-\frac{x^2+y^2}{2\sigma^2}} \right)+
> \frac{\part}{\part y} \left(\frac{-y}{\sigma^2}e^{-\frac{x^2+y^2}{2\sigma^2}} \right)
> \\
> &= \left(\frac{x^2}{\sigma^4} - \frac{1}{\sigma^2}  \right)e^{-\frac{x^2+y^2}{2\sigma^2}} +
> \left(\frac{y^2}{\sigma^4} - \frac{1}{\sigma^2}  \right)e^{-\frac{x^2+y^2}{2\sigma^2}}
> \\
> &= 
> \left(\frac{x^2 + y^2 - 2\sigma^2}{\sigma^4}  \right)e^{-\frac{x^2+y^2}{2\sigma^2}}
> \end{align}
> $$

![](./img/Lec4/LoG.png)

LoG 不是一个 Decomposable 的核，其 Computation Cost 会很高。

### DoG: Difference of Gaussian

我们可以用两个高斯核去近似一个 LoG：
$$
D_G(x, y) = \frac{1}{2\pi\sigma_1^2}e ^{-\frac{x^2+y^2}{2\sigma_1^2}}
- \frac{1}{2\pi\sigma_2^2}e ^{-\frac{x^2+y^2}{2\sigma_2^2}}
\\\\
\approx
\\\\
\nabla^2G(x, y) = \left(\frac{x^2 + y^2 - 2\sigma^2}{\sigma^4}  \right)e^{-\frac{x^2+y^2}{2\sigma^2}}
\\
\text{where } \sigma^2 = \frac{\sigma^2_1\sigma^2_2}{\sigma^2_1 - \sigma_2^2}\ln \frac{\sigma^2_1}{\sigma^2_2}
$$
![](./img/Lec4/DoG_Approx_LoG.png)

上图展示了近似效果。橙线为 LoG，蓝线为 DoG。形状相似，但是尺度不一致。可以 Scale 或者 标准化 一下。

而使用 DoG 可以将两个 Gaussian Kernel 进行 decomposition。

## Group Edges / Link Edges

> 参阅 https://www.bilibili.com/video/BV1168weEEKZ

边缘检测告诉我们对于边缘 Pixel-based 的表示。其不是对于物体 Boundary 的 Geometrical Representation。（即，没有连成图形）

最简单有效的方法是使用 Heuristic Search（启发性搜索）。算法从一个边缘像素开始，尝试添加其周围像素（基于周围像素的边缘强度和方向）

### Local Method

![](./img/Lec4/local_method.png)

我们有很多 Approach：
$$
\begin{align}
| M(x_i, y_j) - M(x_j, y_j) | &\leq \theta_M\\
|\phi(x_i, y_j) -\phi(x_j, y_j)\mid &\leq \theta_\phi
\end{align}
$$
链接的两个 Edge 必须有相近的角度 $\phi$。需要注意是这里的 $\phi \in (-\pi, \pi)$

