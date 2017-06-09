---
title: 数论入门
layout: page
category: cryptology-course
---
## 单向函数和单向陷阱门函数

## 费马定理
若p是素数，a是正整数且不能被p整除，则$a^{p-1}mod \ p = 1$。

### 等价形式
$a^p \equiv a \ mod \ p$，p是素数。

### 证明

## 欧拉定理

### 欧拉函数
$\phi(n)$是比n小且与n互素的正整数个数。

p和q是素数，$n = p*q$，$\phi(n) = \phi(p)\phi(q) = (p-1)(q-1)$。

显然对于素数p，$\phi(n) = p-1$。

### 欧拉定理
对于任意互素的a和n有：

$$\begin{gather}
a^{\phi(n)} \equiv 1 (mod \ n) \\
a^{\phi(n)+1} \equiv a (mod \ n)
\end{gather}
$$

### 证明

## 中国剩余定理

## 离散对数
