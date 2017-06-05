---
title: 信道容量习题
layout: page
category: wiki
---

## 输出的预处理

### a)
$$
\begin{align*}
&\because \widetilde Y = g(Y)  \\
&\therefore p(g(Y)|X,Y)=p(g(Y)|Y)  \\
&\therefore X \rightarrow Y \rightarrow \widetilde Y  \\
&\therefore I(X;Y) \ge I(X;\widetilde Y)\\
\end{align*}
$$

### b)
取等号时，$I(X;Y) = I(X;\widetilde Y)$，即$g(Y)$与$Y$一一对应。

## 二元对称信道的串联

### 数学归纳法

$$
\frac{1}{2}(1-(1-2p)^n)(1-p)+(1-\frac{1}{2}(1-(1-2p)^n))p=\frac{1}{2}(1-(1-2p)^{n+1})
$$

### 推导计算

$$
\begin{gather}
P_{n+1}=P(1-P_n)+(1-P)P_n \\
P_1=P
\end{gather}
$$

通过递推公式直接算出这个等比数列的通项。

### 二项展开
当$x=p,y=1-p$时，$(x+y)^n$的二项展开的奇次项之和：

$$
p^{'}=\frac{1}{2}(x+y)^n-\frac{1}{2}(y-x)^n=\frac{1}{2}(1-(1-2p)^n)
$$

## 时变信道

## 在输出Y上带两个独立观察的信道

## 瓶颈信道

## 信道的选取

## 联合典型性
