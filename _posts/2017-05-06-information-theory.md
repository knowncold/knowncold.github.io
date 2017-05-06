---
title: 信息论笔记
layout: page
category: wiki
---

网络安全专业选修课，田园大师授课  
每节课的笔记，顺便练一练\[\LaTeX\]

## 先修知识补充

线性代数，微积分和概率

### 凸函数

#### 参考
[这么早就说Hessian矩阵是半正定的，会不会给人一种凸函数的感觉？](https://www.yangzhou301.com/2016/03/14/826442654/)

## 熵

熵是随机变量不确定度的度量，当一个随机变量是确定的时候，熵就等于零了。
一个随机变量\[X\]的熵\[H(X)\]定义为

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
- \[H(X)\ge0\]
- \[H_b(X) = (log_ba)H_a(X)\]

### 联合熵 (joint entropy)

对于服从联合分布为\[p(x,y)\]的一对离散随机变量\[(X,Y)\]

$$ H(X,Y) = - \sum_{x\in\mathcal{X}} \sum_{y\in\mathcal{Y}} p(x,y)log \ p(x,y) $$

### 条件熵 (conditional entropy)

$$ H(Y|X) = -Elog \ p(Y|X) $$

### 链式法则

$$ H(X,Y) = H(X) + H(Y|X) $$

#### 推论

$$ H(X,Y|Z) = H(X|Z) + H(Y|X,Z) $$

## 互信息量

$$ I(X;Y) = E_{p(x,y)} log \frac{p(X,Y)}{p(X)p(Y)} $$
