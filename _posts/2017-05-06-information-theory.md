---
title: 信息论笔记
layout: page
category: wiki
---

网络安全专业选修课，田园大师授课  
每节课的笔记，顺便练一练 $\LaTeX$

## 先修知识补充

线性代数，微积分和概率

### 凸函数

#### 参考
[这么早就说Hessian矩阵是半正定的，会不会给人一种凸函数的感觉？](https://www.yangzhou301.com/2016/03/14/826442654/)

## 熵

熵是随机变量不确定度的度量，当一个随机变量是确定的时候，熵就等于零了。
一个随机变量$X$的熵$H(X)$定义为

$$ H(X) = - \sum_{x\in\mathcal{X}} p(x)log \ p(x) $$

### 熵的最大值

相当于求

$$
\begin{align}
\max	&-\sum_{i=1}^n p_i \ \ln p_i \\
s.t.	&\sum_{i=1}^n p_i = 1 \quad p_i \gt 0 \\
\end{align}
$$


#### 引理
- $H(X)\ge0$
- $H_b(X) = (log_ba)H_a(X)$

### 联合熵 (joint entropy)

对于服从联合分布为\[p(x,y)\]的一对离散随机变量\[(X,Y)\]

$$ H(X,Y) = - \sum_{x\in\mathcal{X}} \sum_{y\in\mathcal{Y}} p(x,y)log \ p(x,y) $$

### 条件熵 (conditional entropy)

$$ H(Y|X) = -Elog \ p(Y|X) $$

### 链式法则

$$
\begin{align}
H(X,Y) = H(X) + H(Y|X) \\
H(X,Y) = H(Y) + H(X|Y)
\end{align}
$$

#### 推论

$$ H(X,Y|Z) = H(X|Z) + H(Y|X,Z) $$

## 互信息量

$$ I(X;Y) = E_{p(x,y)} log \frac{p(X,Y)}{p(X)p(Y)} $$

$$
\begin{align}
I(X;Y) &= \sum_{x\in\mathcal{X},y\in\mathcal{Y}} p_{xy}log \frac{p_{y|x}}{p_y} \\
		&= H(Y) - H(Y|X)	\\
		&= H(X) - H(X|Y)
\end{align}		
$$

当信息量等于零的时候，两者独立

## 马尔克夫链

### 概念

随机过程和随机序列

$$ \cdots X_{-2}, X_{-1}, X_0, X_1, X_2, \cdots $$

随机序列描述的是$X_n$在不同时间片有不同的概率分布

假如服从马尔克夫链，则满足

$$ P(X_n | X_{n-1}, X_{n-2}, \cdots, X_{n-k}) = P(X_n | X_{n-1}, X_{n-2}, \cdots, X_{n-k}, X_{n-k-1}) $$

即通过过去的时间片的概率分布来预测未来的某个时间片的概率分布时，历史更多也没意义

### 定义

$$
\begin{gather}
\forall \ n > n_1 > n_2 > \cdots > n_k:    \\
P(X_n | X_{n1}, X_{n2}, \cdots, X_{nk}) = P(X_n | X_{n1})
\end{gather}
$$

即通过过去预测未来时，只与最近的那个时间片有关，与之前的时间片都无关

#### 联合概率密度

$$
\begin{align}
P(X_3, X_2, X_1) &= P(X_3 | X_2) P(X_2 | X_1) P(X_1)	\\
P(X_n, \cdots , X_1) &= P(X_n | X_{n-1}) P(X_{n-1} | X_{n-2}) \cdots P(X_2 | X_1) P(X_1) \\
\end{align}
$$

### 子列

对于马尔克夫链的子列

若满足$X_1 \to X_2 \to X_3$，通过基本性质，显然有$P_{X3 | X2,X1} = P_{X3 | X2}$  
可以推出$X_3 \to X_2 \to X_1$，即时间片在时间相反的情况下也满足马尔克夫链

#### 证明

$$
P(X_1 | X_2, X_3) = \frac{P(X_1, X_2, X_3)}{P(X_2, X_3)} = \frac{P(X_3 | X_2, X_1) P(X_2, X_1)}{P(X_2)P(X3 | X_2))} = P(X_1 | X_2)
$$

### 数据处理不等式

若$X_1 \to X_2 \to X_3$，则有$I(X_1 ; X_2) \ge I(X_1 ; X_3)$。  
即对于同一个时间片，相距越近的时间片的互信息量不小于相距越远的信息量。

#### 证明

$$
\begin{align}
&\; I(X_1 ; X_2) - I(X_1; X_3) \\
&= \sum_{X1,X2} P(X1,X2) log \frac{P(X1,X2)}{P(X1)P(X2)} -\sum_{X1,X3} P(X1,X3) log \frac{P(X1,X3)}{P(X1)P(X3)} \\
&= \sum_{X1,X2,X3} P(X1,X2,X3) log \frac{P(X1,X2)}{P(X1)P(X2)} -\sum_{X1,X3,X2} P(X1,X3,X2) log \frac{P(X1,X3)}{P(X1)P(X3)} \\
&= \sum_{X1,X2,X3} P(X1,X2,X3) log \frac{P(X1,X2) P(X3)}{P(X1,X3)P(X2)} \\
&= \sum_{X1,X2,X3} P(X1,X2,X3) log \frac{P(X2)P(X1 | X2) P(X3)}{P(X3)P(X1 | X3)P(X2)} \\
&= \sum_{X1,X2,X3} P(X1 | X2,X3) P(X2,X3) log \frac{P(X1 | X2)}{P(X1 | X3)} \\
&= \sum_{X2,X3}P(X2,X3)(\sum_{X1} P(X1|X2)log \frac{P(X1 | X2)}{P(X1 | X3)})
\end{align}
$$
