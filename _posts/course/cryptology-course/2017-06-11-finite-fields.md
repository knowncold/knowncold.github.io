---
title: 有限域
layout: page
category: cryptology-course
---

## 群环域

## 模运算
$a \equiv b \pmod n$

$a \pmod n = b \pmod n$

### 同余的性质
如果$n|(a-b)$，则$a \equiv b \pmod n$

$a≡b \pmod n$隐含$b≡a \pmod n$

$a≡b \pmod n$和$b≡c \pmod n$隐含$a≡c \pmod n$

$(a+b) \pmod n = (a \pmod n + b \pmod n) \pmod n$

### 加法逆元和乘法逆元

#### 加法逆元

对每一个$w \in Z_n$，存在一个$z$，使得$w+z \equiv 0 \pmod n$，$z$即为加法逆元$-w$。

#### 乘法逆元
对每一个w∈Zp，存在一个z，使得wxz≡1 mod p，p为素数， w与p互素，则z即为乘法逆元w-1
因为w与p互素，如果用w乘以Zp中的所有数模p，得到的余数将以不同次序涵盖Zp中的所有数，那么至少有一个余数的值为1。因此，在Zp中的某个数与w相乘模p的余数为1, 这个数就是w的乘法逆元, w-1
某些但非全部整数存在一个乘法逆元就将使模数不再是素数。如果gcd(a, n)=1, 则能在Zn中找到b，使得axb ≡1 mod n，则b即为乘法逆元a-1, 因为a与n互素

## 欧几里得算法
对于任何非负的整数a和n，gcd(a, n)=gcd(n mod a, a)

### 证明
假设我们有整数a,b使得d=gcd(a,b)。假设a≥b>0，现在用b除a，由除法可得到a=q1b+r1    0≤r1<b 如果恰巧r1=0，则b|a且d=gcd(a,b)=b。但是如果r1≠0,我们说d|r1。这基于除法的基本性质：由d|a和d|b可以推出d|(a-q1b)，即d|r1。现在假设有任意的整数c整除b和r1.因此c|(q1b+r1)=a。因此c同时整除a和b，必须有c≤d，而d是a和b的最大公因子。因此d=gcd(b,r1)。

a=q1b+r1    0≤r1<b
假设r1≠0.因为b>r1，可以用r1除b，应用除法有
b=q2r1+r2      0≤r2<r1
如前所述，如果r2=0，则d=r1.如果r2≠0，则d=gcd(r1,r2)。
继续除法过程直到余数为0，比如说在第(n+1)阶段有rn整除
r(n-1)。结果为如下的方程系统：

[有一个图]()

使d等于gcd(a,b),根据gcd的定义，有d|a和d|b成
立。对于任意正整数b,a可以表示为如下形式：
a=kb+r ≡r(mod b)
a mod b=r
其中k,r为整数。因此，对某个整数k,有(a mod b)=a-kb.
因为d|b，所以有d|kb；又因为d|a，所以有d|(a mod b). 这说
明d是b和(a mod b)的公因子。反之，如果d是b和(a mod b)
的公因子，那么d|kb并且由此可知d|[kb+(a mod b)],即d|a.
因此，a和b的公因子的集合与b和(a mod b)的公因子的集合
相等。

## 扩展欧几里得算法
对于给定的整数a和b，扩展的Euclid算法不仅计算出最大公
因子d，而且还有另外的整数x和y，它们满足如下方程：
ax+by=d=gcd(a,b)
用扩展的Euclid算法计算(x,y,d).
假设在每一步骤i都可以找到xi和yi满足ri=axi+byi。

[有一个图]()

从原始的Euclid算法知该过程当余数为0时结束，求得a和b
的最大公因子为d=gcd(a,b)=rn. 我们也决定了

[有一个图]()
