# Lecture 10: Feature Tracking

## Tracking in Image Sequences

对于图片序列，我们可以认为有三个轴 $(x, y, t)$。​我们有如下有下面三个基本假设：

1. **亮度恒定（Brightness Constancy）**：一个物体点在不同帧之间的亮度保持不变

2. **时间持续性（Temporal Persistence）**：特征在连续帧之间的运动较小,且相机位置变化缓慢

3. **空间相干性（Spatial Coherence）**：相邻的特征属于同一物理表面，具有相似的运动

恒定性可以表示为:
$$
I(x+\underbrace{\Delta x}_u, y + \underbrace{\Delta y}_v, t + \underbrace{\Delta t}_{\approx 1}) = I(x, y, t)
$$
我们可以简化 $\Delta t = 1$​

基于亮度恒常性，我们可以使用泰勒级数展开来建立沿 x、y、t 轴的强度梯度之间的关系：
$$
\begin{align*}
I(x+u, y+v, t+1) &= I(x, y, t)\\
\because I(x+u, y+v, t+1) &\approx I(x, y, t) +\left[
\frac{\part I}{\part x}u +
\frac{\part I}{\part y}v +
\frac{\part I}{\part t} +
\right] +\cdots\\ 
\therefore I(x+u, y+v, t+1) - I(x, y, t) &=
\frac{\part I}{\part x}u +
\frac{\part I}{\part y}v +
\frac{\part I}{\part t} = 0 \\
\frac{\part I}{\part x}u +
\frac{\part I}{\part y}v +
\frac{\part I}{\part t} &= 0 \\
I_x u + I_y v + I_t &= 0 \\
I_x u + I_y v &= -I_t \\
\begin{bmatrix}I_x & I_y\end{bmatrix}
\begin{bmatrix}u \\ v\end{bmatrix} &= -I_t
\end{align*}
$$

> 相邻图像帧之间需要恢复的位移 ($t$​)；一个方程有两个未知数，因此不可能解出。但是，如果我们知道有几个点一起移动，我们可以用最小二乘法来解决这个问题。


$$
\begin{align*}
\begin{bmatrix}
I_x(p_1) & I_y(p_1)\\
I_x(p_2) & I_y(p_2)\\
\vdots   & \vdots \\
I_x(p_N) & I_y(p_N)\\
\end{bmatrix}
\begin{bmatrix}u \\ v\end{bmatrix} &= -

\begin{bmatrix}
I_t (p_1) \\
I_t (p_2) \\
\vdots \\
I_t (p_N)
\end{bmatrix}
\end{align*}
$$
