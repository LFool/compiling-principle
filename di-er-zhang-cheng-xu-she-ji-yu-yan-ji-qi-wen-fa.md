# 第二章 程序设计语言及其文法

> 想让编译系统自动的对语言进行词法、语法、语义分析，就需要把语言的相关知识 ——— **文法**提供给计算机

## 1. 基本概念

### 1.1 字母表

字母表 ∑ 是一个有穷符号集合

* 符号：字母、数字、标点符号、...

#### 运算

* **乘积** $$\sum_1 \sum_2 = \{ ab \mid a \in \sum_1 , b \in \sum_2\}$$ 
  * **例：** $$\{0,1\}\{a,b\} = \{0a, 0b, 1a, 1b\}$$ 
* **n次幂**

$$
\left\{
\begin{aligned}
\sum ^ 0 & =  \{\varepsilon\} \\
\sum ^ n & =  \sum^{n-1}\sum &,n\geq1  \\
\end{aligned}
\right.
$$

* * **例：** $$\{0,1\}^3 = \{0,1\}\{0,1\}\{0,1\} = \{000,001,010,011,100,101,110,111\}$$ 
* **正闭包** $$\sum^+ = \sum \bigcup \sum^2 \bigcup \sum^3 \bigcup \cdots$$ 
  * **例：** $$\{a,b,c,d\}^+ = \{a,b,c,d,aa,ab,ac,ad,ba,bb,bc,bd,...,aaa,aab,aac,aad,aba,abb,abc,\cdots\}$$ 
* **克林闭包** $$\sum^* = \sum^0 \bigcup \sum^+ = \sum^0 \bigcup \sum \bigcup \sum^2 \bigcup \sum^3 \bigcup \cdots$$ 
  * **例：** $$\{a,b,c,d\}^+ = \{\varepsilon,a,b,c,d,aa,ab,ac,ad,ba,bb,bc,bd,...,aaa,aab,aac,aad,aba,abb,abc,\cdots\}$$ 

### 1.2 串

设 $$\sum$$ 是一个字母表， $$\forall x \in \sum ^ *$$ ，x称为是$$\sum$$上的一个串

* 串是字母表中**符号**的一个**有穷序列**

串 s 的长度，通常记为 $$\mid S \mid$$ ，是指 s 中**符号的个数**

* 例： $$\mid aab \mid = 3$$ 

空串是长度为0的串，用  $$\epsilon$$ \(epsilon\) 表示

* $$\mid \epsilon \mid = 0$$ 

#### 运算

* **连接** $$xy$$ 
  * $$x = dog$$ 且 $$y = house$$ ，那么 $$xy = doghouse$$ 
  * $$\epsilon S = S \epsilon = S$$ 
  * 设x, y, z 是三个字符串，如果 x = yz，则称 y 是 x 的前缀，z 是 x 的后缀
* **幂** 

$$
\left\{
\begin{aligned}
S ^ 0 & =  \epsilon \\
S ^ n & =  S ^ {n - 1}S && , n \geq 1 \\
\end{aligned}
\right.
$$

* * $$S^1 = S^0 S = \epsilon S = S，S^2 = SS，S^3 = SSS，\cdots$$ 

## 2. 文法的定义

$$G = (V_T，V_N，P，S)$$ ****

* $$V_T$$ ：终结符集合
  * **终结符**是文法所定义的语言的基本符号，有时也称为token
  * 例： $$V_T = \{ apple，boy，eat，little \}$$ 
* $$V_N$$ ：非终结符集合
  * **非终结符**是用来表示语法成分的符号，有时也称为“语法变量”
  * $$V_T \bigcap V_N = \Phi$$ 
  * $$V_T \bigcup V_N ：文法符号集$$ 
  * 例： $$V_N = \{ <句子>，<名词短语>，<动词短语>，<名词>， \cdots \}$$ 
* $$P$$ ：产生式集合
  * 产生式描述了将终结符和非终结符组合成串的方法
  * 产生式的一般形式： $$\alpha \rightarrow\beta$$  读作： $$\alpha$$ 定义为 $$\beta$$
    * $$\alpha \in (V_T \bigcup V_N) ^ +$$ ，且$$\alpha$$ 中至少包含 $$V_N$$中的一个元素：称为产生式的头部或者左部
    * $$\beta \in (V_T \bigcup V_N) ^ *$$ ，称为产生式的体或右部
  * 例： $$P = \{ <句子> \longrightarrow <名词短语><动词短语>，\cdots \}$$ 
* $$S$$ ：开始符号
  * $$S \in V_N$$ 。开始符号表示的是该文法中最大的语法成分
  * 例： $$S = <句子>$$ 

**例：**

 $$ \begin{aligned} G = (\{&id, +, *, (, ) \}, \{ E \}, P, E) \\  P = \{&E \longrightarrow E + E,  \\   &E \longrightarrow E * E,  \\  &E \longrightarrow(E),  \\  &E \longrightarrow id \}  \end{aligned} $$ 

## 3. 语言的定义

有了文法（语言规则），如果判断一个词串是否满足文法的句子？

* 句子的**推导**（派生） - 从**生成**语言的角度
* 句子的**归约**                 - 从**识别**语言的角度

![](.gitbook/assets/image%20%289%29.png)

**句子和句型**

* 含有非终结符的为句型，只有终结符的为句子

![](.gitbook/assets/image%20%2810%29.png)

**语言的定义**

由文法G的开始符号S推导出的所有句子构成的集合称为文法G生成的语言，记为L\(G\)

## 4. 文法的分类

0型文法

* $$\alpha \rightarrow \beta$$ ： $$\alpha$$ 中至少包含一个非终结符

1型文法（上下文有关文法）

* $$\alpha \rightarrow\beta$$ ： $$\forall \alpha \rightarrow \beta \in P，\mid \alpha \mid \leq \mid \beta \mid$$ 

2型文法（上下文无关文法）

* $$\alpha \rightarrow\beta$$ ： $$\alpha \in V_N$$ 

3型文法（正规文法、有限自动机）

* $$\alpha \rightarrow\beta$$  
  * 右线性文法： $$A \rightarrow wB 或 A \rightarrow w$$ 
  * 左线性文法： $$A \rightarrow Bw 或 A \rightarrow w$$ 

## 5. CFG的分析树

![](.gitbook/assets/image%20%288%29.png)

### 5.1 二义性文法

如果一个文法可以为某个句子生成多棵分析树，则称这个文法是二义性的

**判定：**对于任意一个上下文无关文法，不存在一个算法，判定它是无二义性的；但可以给出一组充分条件，满足这组充分条件的文法是无二义性的

* 满足，肯定无二义性
* 不满足，不一定有二义性

