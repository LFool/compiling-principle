# 第二章 程序设计语言及其文法



> 想让编译系统自动的对语言进行词法、语法、语义分析，就需要把语言的相关知识 ——— **文法**提供给计算机



## 1. 基本概念



### 字母表

字母表 ∑ 是一个有穷符号集合

- 符号：字母、数字、标点符号、...



#### 运算

1. **乘积**
   $$
   \sum_1 \sum_2 = \{ ab \mid a \in \sum_1 , b \in \sum_2\}
   $$
   

   **例：**
   $$
   \{0,1\}\{a,b\} = \{0a, 0b, 1a, 1b\}
   $$
   

   

2. **n次幂**
   $$
   \left\{
   \begin{aligned}
   \sum ^ 0 & =  \{\varepsilon\} \\
   \sum ^ n & =  \sum^{n-1}\sum &,n\geq1  \\
   \end{aligned}
   \right.
   $$
   

   **例：**
   $$
   \{0,1\}^3 = \{0,1\}\{0,1\}\{0,1\} = \{000,001,010,011,100,101,110,111\}
   $$
   

3. **正闭包**
   $$
   \sum^+ = \sum \bigcup \sum^2 \bigcup \sum^3 \bigcup \cdot\cdot\cdot
   $$
   

   **例：**
   $$
   \{a,b,c,d\}^+ = \{a,b,c,d,aa,ab,ac,ad,ba,bb,bc,bd,...,aaa,aab,aac,aad,aba,abb,abc,...\}
   $$
   

4. **克林闭包**
   $$
   \sum^* = \sum^0 \bigcup \sum^+ = \sum^0 \bigcup \sum \bigcup \sum^2 \bigcup \sum^3 \bigcup \cdot\cdot\cdot
   $$
   

   **例：**
   $$
   \{a,b,c,d\}^+ = \{\varepsilon,a,b,c,d,aa,ab,ac,ad,ba,bb,bc,bd,...,aaa,aab,aac,aad,aba,abb,abc,...\}
   $$



### 串

设 $\sum$