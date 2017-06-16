---
title: 信道容量习题
layout: page
category: information-theory
---

## 1 输出的预处理

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

## 7 二元对称信道的串联

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

## 11 时变信道

## 在输出Y上带两个独立观察的信道

## 25 瓶颈信道

$X \rightarrow V \rightarrow Y$

$$
I(X;Y) \le I(V;Y) = H(V) - H(V|Y) \le H(V) \le -\sum_{i=1}^k \frac{1}{k} log \frac{1}{k} = log k
$$

## 28 信道的选取
参看习题2.10

### 证明 $2^C=2^{C_1}+2^{C_2}$

$$
P_{y|x}=
\begin{cases}
P_{y|x}^1	x \in X_1, y \in Y_1	\\
P_{y|x}^2	x \in X_2, y \in Y_2	\\
\end{cases}	\\
P_x=
\begin{cases}
\begin{align*}
\alpha P_x^1 \quad &x\in X_1	\\
(1-\alpha) P_x^2 \quad &x\in X_2\\
\end{align*}
\end{cases}	\\
$$

$$
\begin{align*}
P_y &= \sum_{x \in X} P_{y|x}P_x \\
&= \sum_{x \in X_1} P_{y|x}^1 \alpha P_x^1 + \sum_{x \in X_2} P_{y|x}^2 (1-\alpha) P_x^2	\\
&= \alpha P_y^1 + (1-\alpha) P_y^2
\end{align*}
$$

$$
\begin{align*}
H(Y) &= - \sum_{y \in Y_1} \alpha (P_y^1 log P_y^1 + log \alpha) - \sum_{y \in Y_2} (1-\alpha) (P_y^2 log P_y^2 + log (1-\alpha))	\\
&= \alpha H(Y_1) +(1-\alpha) H(Y_2) - H(\alpha)
\end{align*}
$$

$$
\begin{align*}
H(Y|X) &= -\sum_{x \in X_1} \alpha P_x^1 P_{y|x}^1 log P_{y|x}^1 -\sum_{x \in X_2} (1-\alpha) P_x^2 P_{y|x}^2 log P_{y|x}^2	\\
&= \alpha H(Y_1|X_1) + (1-\alpha) H(Y_2|X_2)
\end{align*}
$$

$$
\begin{align*}
I(X;Y)
&= H(Y) - H(Y|X)	\\
&= \alpha I(X_1;Y_1) + (1 - \alpha) I(X_2;Y_2) - H(\alpha)
\end{align*}	\\
$$

$$
I(X;Y) \le \alpha I(X_1;Y_1) + (1 - \alpha) I(X_2;Y_2) - H(\alpha) = C_{\alpha}
$$

$$
\frac{d(C_{\alpha})}{d(\alpha)}=0	\\
\alpha = \frac{e^{C_1}}{e^{C_1}+e^{C_2}}	\\
C = log_2(2^{C_1}+2^{C_2})	\\
P_x^* = \alpha P_x^{1*} + (1-\alpha)P_x^{2*}	\\
2^C=2^{C_1}+2^{C_2}
$$

其中  
$H(\alpha)=(\alpha log \alpha + (1-\alpha)log(1-\alpha))$  

$P_x^{1*}$满足$C_1$的信道容量情况  

$P_x^{2*}$满足$C_2$的信道容量情况

当其中一个信道容量远大于另一个时，比如$C_1 \gg C_2$，从$\alpha$的表示可以看出此时趋向于1，即会倾向于选择这个大容量的信道，导致最终$C_{max} \approx C_1$

#### 猜测n个信道可以选择时的情况

$$
C = log_e(\sum_{i=1}^n e^{C_i})
$$

## 联合典型性
