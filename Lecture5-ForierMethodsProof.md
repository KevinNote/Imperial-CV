# Mathematical Proof of DFT

[TOC]

## FT Different Forms & Euler Formula

### Sine-cosine form

$$
f(x)=\sum^\infty_{h=0} \left[a_h \cos\left(2\pi h\frac{x}{N}\right) + b_h \sin\left(2\pi h\frac{x}{N}\right)\right]\\
$$

### Amplitude-phase form

$$
f(x)=\frac{A_0}{2} + \sum^N_{h=1}\left( A_h \cos\left(2\pi h \frac{x}{N}-\varphi_h\right) \right)
$$

### Exponential form

$$
f(x) = \sum^N_{h=-N} c_h e^{i2\pi h \frac{x}{N}}
$$

### Euler formula

$$
e^{i\theta} = \cos \theta + i\sin\theta\\
\cos\theta=\frac{e^{i\theta} + e^{-i\theta}}{2}\\
\sin \theta=\frac{e^{i\theta} + e^{-i\theta}}{2i}
$$

## Proof

令 $\gamma = 2\pi h \frac{x}{N}$

### Sine-cosine form to Amplitude-phase form

考虑 Amplitude (Magnitude) 与 Phrase (Direction) 于 $h$ 时的定义：
$$
\begin{cases}
A_h = \sqrt{a_h^2 + b_h^2}\\
\varphi_h = \tan^{-1}\left(\frac{b_h}{a_h}\right)
\end{cases}
$$
则有
$$
\begin{cases}
a_h = A_h \cos(\varphi_h)\\
b_h = A_h \sin(\varphi_h)
\end{cases}
$$

带入 Sin-cons form
$$
\begin{align}
& a_h \cos(\gamma) + b_h \sin(\gamma)
\\ =& A_h \cos(\varphi_h) \cos(\gamma) + A_h \sin(\varphi_h) \sin(\gamma)
\\ =& A_h[\cos(\varphi_h) \cos(\gamma) + \sin(\varphi_h) \sin(\gamma)]
\end{align}
$$
考虑三角恒等变换
$$
\cos(\alpha-\beta) = \cos \alpha \cos \beta + \sin \alpha \sin \beta
$$
则有
$$
\begin{align}
& a_h \cos(\gamma) + b_h \sin(\gamma)
\\ =& A_h[\cos(\varphi_h) \cos(\gamma) + \sin(\varphi_h) \sin(\gamma)]
\\ =& A_h[\cos(\varphi_h -\gamma)]
\\ =& A_h[\cos(\gamma-\varphi_h)]
\end{align}
$$
考虑完整 Series

- $h=0$，考虑 Sine-cosine form：
  $$
  \begin{align}
  f(x)_0 &= a_0\cos(0) + b_0\sin(0) \\
  &= a_0 \cdot 1 + b_0 \cdot 0 \\
  &= a_0
  \end{align}
  $$
  而 $A_0 = |a_0|$，看似不成立，但是如考虑公式意义，则成立。

  这是因为在傅里叶级数展开中，对于 $h\geq1$ 的项： $A_h\cos(\gamma - \varphi_h)$ 的振幅 $A_h$ 实际上是原信号在该频率分量上峰-峰值的一半。

  为了保持一致性，令系数为 $1/2$

- $h\geq 1$，则有
  $$
  f(x) = \frac{A_0}{2} + \sum_{h=1}^N A_h \cos(\gamma - \varphi_h)
  $$

$$
\begin{align*} \square \end{align*}
$$

### Sine-cosine form to Exponential form

$$
\begin{align*}
\text{Given: }
&f(x)=\sum^\infty_{h=0} [a_h \cos(\gamma) + b_h \sin(\gamma)]\\
&e^{i\theta} = \cos \theta + i\sin\theta\\
&\cos\theta=\frac{e^{i\theta} + e^{-i\theta}}{2}\\
&\sin\theta=\frac{e^{i\theta} - e^{-i\theta}}{2i}\\
\text{Want: }
&f(x) = \sum^N_{h=-N} c_h e^{i\gamma}
\end{align*}
$$

应用欧拉公式，有：
$$
\begin{align}
f(x)
&=\sum^\infty_{h=0} [a_h \cos(\gamma) + b_h \sin(\gamma)]\\
&= \sum_{h=0}^{\infty} \left[a_h\left(\frac{e^{i\gamma} + e^{-i\gamma}}{2}\right) + b_h\left(\frac{e^{i\gamma} - e^{-i\gamma}}{2i}\right)\right]
\\
&= \sum_{h=0}^{\infty} \left[\frac{a_h}{2}\left(e^{i\gamma} + e^{-i\gamma}\right) + \frac{b_h}{2i}\left(e^{i\gamma} - e^{-i\gamma}\right)\right]
\\
&= \sum_{h=0}^{\infty} \left[\left(\frac{a_h}{2} + \frac{b_h}{2i}\right)e^{i\gamma} + \left(\frac{a_h}{2} - \frac{b_h}{2i}\right)e^{-i\gamma}\right]
\end{align}
$$
定义：
$$
\begin{cases}
c_{h} = \frac{a_h}{2} + \frac{b_h}{2i}\\
c_{-h} = \frac{a_h}{2} - \frac{b_h}{2i}
\end{cases}
$$

> 这里的 $h$ 和 $-h$ 理解为下标，而不应该理解为值。

则有
$$
\begin{align}
f(x)
&= \sum_{h=0}^{\infty} \left[\left(\frac{a_h}{2} + \frac{b_h}{2i}\right)e^{i\gamma} + \left(\frac{a_h}{2} - \frac{b_h}{2i}\right)e^{-i\gamma}\right]
\\
&= \sum_{h=0}^{\infty} \left[c_h e^{i\gamma} + c_{-h} e^{-i\gamma}\right]
\\
&= \sum_{h=-\infty}^{\infty} c_h e^{i\gamma}
\end{align}
$$

$$
\begin{align*} \square \end{align*}
$$
