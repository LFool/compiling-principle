# 第二章 程序设计语言及其文法

> 想让编译系统自动的对语言进行词法、语法、语义分析，就需要把语言的相关知识 ——— **文法**提供给计算机

## 1. 基本概念

### 字母表

字母表 ∑ 是一个有穷符号集合

* 符号：字母、数字、标点符号、...

#### 运算

**乘积** $$\sum_1 \sum_2 = \{ ab \mid a \in \sum_1 , b \in \sum_2\}$$ 

* **例：** $$\{0,1\}\{a,b\} = \{0a, 0b, 1a, 1b\}$$ 

**n次幂**

$$
\left\{
\begin{aligned}
\sum ^ 0 & =  \{\varepsilon\} \\
\sum ^ n & =  \sum^{n-1}\sum &,n\geq1  \\
\end{aligned}
\right.
$$

* **例：** $$\{0,1\}^3 = \{0,1\}\{0,1\}\{0,1\} = \{000,001,010,011,100,101,110,111\}$$ 

**正闭包** $$\sum^+ = \sum \bigcup \sum^2 \bigcup \sum^3 \bigcup \cdots$$ 

* **例：** $$\{a,b,c,d\}^+ = \{a,b,c,d,aa,ab,ac,ad,ba,bb,bc,bd,...,aaa,aab,aac,aad,aba,abb,abc,\cdots\}$$ 

**克林闭包** $$\sum^* = \sum^0 \bigcup \sum^+ = \sum^0 \bigcup \sum \bigcup \sum^2 \bigcup \sum^3 \bigcup \cdots$$ 

* **例：** $$\{a,b,c,d\}^+ = \{\varepsilon,a,b,c,d,aa,ab,ac,ad,ba,bb,bc,bd,...,aaa,aab,aac,aad,aba,abb,abc,\cdots\}$$ 

### 串

设 $$\sum$$ 是一个字母表， $$\forall x \in \sum ^ *$$ ，x称为是$$\sum$$上的一个串

* 串是字母表中**符号**的一个**有穷序列**

串 s 的长度，通常记为 $$\mid S \mid$$ ，是指 s 中**符号的个数**

* 例： $$\mid aab \mid = 3$$ 

空串是长度为0的串，用  $$\epsilon$$ \(epsilon\) 表示

* $$\mid \epsilon \mid = 0$$ 

#### 运算

**连接** $$xy$$ 

* $$x = dog$$ 且 $$y = house$$ ，那么 $$xy = doghouse$$ 
* $$\epsilon S = S \epsilon = S$$ 
* 设x, y, z 是三个字符串，如果 x = yz，则称 y 是 x 的前缀，z 是 x 的后缀

**幂** 

$$
\left\{
\begin{aligned}
S ^ 0 & =  \epsilon \\
S ^ n & =  S ^ {n - 1}S && , n \geq 1 \\
\end{aligned}
\right.
$$

* $$S^1 = S^0 S = \epsilon S = S，S^2 = SS，S^3 = SSS，\cdots$$ 

## 2. 文化的定义



