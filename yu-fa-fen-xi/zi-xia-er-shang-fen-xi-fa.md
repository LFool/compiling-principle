# 自下而上分析法

## 1. 自下而上分析法思路

* 从输入串开始，逐步进行归约，直到文法的开始符号
* 归约：根据文法的产生式规则，把产生的右部替换成左部符号
* 从树的末端开始，构造语法树

采用一个寄存符号的先进后出的栈，把输入符号一个一个地移动进栈里，把栈顶形成某个产生式的候选式时，即把栈顶的这一部分替换成该产生式的左部符号

## 2. 自上而下分析的基本问题

**识别可归约串**

当栈顶的符号串可以匹配多个候选式的时候，选择哪一个候选式进行归约成为了该分析法的一个基本问题

## 3. 短语、直接短语、句柄

对于一个文法 $$G$$ ， $$S$$ 是一个文法的开始符号，假定 $$\alpha\beta\delta$$ 是文法 $$G$$ 的一个句型，如果有 $$S \stackrel{*}\Rightarrow \alpha A \delta ，A \stackrel{+}\Rightarrow \beta$$ ，则称 $$\beta$$ 是句型 $$\alpha \beta\delta$$ 相对于非终结符 $$A$$ 的**短语**

特别的，如果有 $$A \Rightarrow \beta$$ ，则称 $$\beta$$ 是句型 $$\alpha\beta\delta$$ 相对于规则 $$A \rightarrow \beta$$ 的**直接短语**

一个句型的最左直接短语称为该句型的**句柄**

* 以某非终结符为根的两代以上的子树的所有末端结点从左到右排列就是相对于该非终结符的一个**短语**
* 如果子树只有两代，则该短语就是**直接短语**

**例子：**文法 ****$$G(E)$$ 

                    ****$$E \rightarrow T \mid E + T \\ T \rightarrow F \mid T * F \\ F \rightarrow (E) \mid i$$                                        ****![](../.gitbook/assets/image%20%2867%29.png) ****

短语： $$i_1，i_2，i_3，i_1 * i_2，i_1 * i_2 + i_3$$ 

直接短语： $$i_1，i_2，i_3$$ 

句柄： $$i_1$$ 

## 4. 规范归约

定义：假设 $$\alpha$$ 是文法 $$G$$ 的一个句子，我们称序列 $$\alpha_n，\alpha_{n-1}，\dots， \alpha_0$$ 是 $$\alpha$$ 的一个规范归约，当且仅当：

1. $$\alpha_n = \alpha$$ 
2. $$\alpha_0$$ 为文法的开始符号，即 $$\alpha_0 = S$$ 
3. 对于任何 $$i$$ ， $$0 \le i \le n$$ ， $$\alpha_{i-1}$$ 是从 $$\alpha_i$$ 经把句柄替换成相应产生式左部符号而得到的

### 4.1 规范归约与规范推导

* **最右推导**是**规范推导**
* **规范归约**是**最右推导**的逆过程

### 4.2 符号栈的使用

          $$E \rightarrow T \mid E + T \\ T \rightarrow F \mid T * F \\ F \rightarrow (E) \mid i$$         输入串： $$i * i + i$$ 

| 步骤 | 符号栈 | 输入串 | 动作 |
| :---: | :---: | :---: | :---: |
| 0 | $$\#$$  | $$i*i+i \#$$  | 预备 |
| 1 | $$\# i$$  | $$*i+i \#$$  | 进 |
| 2 | $$\# F$$  | $$*i+i \#$$  | 归，用 $$F \rightarrow i$$  |
| 3 | $$\# T$$  | $$*i+i \#$$  | 归，用 $$T \rightarrow F$$ |
| 4 | $$\# T*$$  | $$i+i \#$$  | 进 |
| 5 | $$\# T*i$$  | $$+i \#$$  | 进 |
| 6 | $$\# T*F$$  | $$+i \#$$  | 归，用 $$F \rightarrow i$$ |
| 7 | $$\# T$$  | $$+i \#$$  | 归，用 $$T \rightarrow T*F$$ |
| 8 | $$\# E$$  | $$+i \#$$  | 归，用 $$E \rightarrow T$$ |
| 9 | $$\# E+$$  | $$i \#$$  | 进 |
| 10 | $$\# E+i$$ | $$\#$$  | 进 |
| 11 | $$\# E + F$$  | $$\#$$  | 归，用 $$F \rightarrow i$$ |
| 12 | $$\# E + T$$  | $$\#$$  | 归，用 $$T \rightarrow F$$ |
| 13 | $$\# E$$  | $$\#$$  | 归，用 $$E \rightarrow E+T$$ |
| 14 | $$\# E$$  | $$\#$$  | 接收 |

上述方法采用人工判断句柄，然后进行归约的过程

## 5. 算符优先分析算法

对于一个文法，如果它的句子可能有好几种不同的规范归约，如果规定算符的优先次序，并按照这个规定进行归约，则归约过程是唯一的

### 5.1 算符优先分析基本思想

* 起决定作用的是相邻的两个**算符**（终结符）之间的**优先关系**
* 所谓**算符优先分析法**就是定义**算符**（终结符）之间的某种**优先关系**，借助于这种关系寻找“可归约串”和进行归约

### 5.2 优先关系

对于任何两个相继出现的终结符 $$a$$ 与 $$b$$ 的三种优先关系

* $$a \lessdot b$$ ，$$a$$ 的优先级低于 $$b$$ 
* $$a \eqcirc b$$ ，$$a$$ 的优先级等于 $$b$$
* $$a \gtrdot b$$ ，$$a$$ 的优先级高于 $$b$$ 

### 5.3 算符优先文法

**算符文法：**

一个文法，如果它的任一产生式的右部都不含两个相继（并列）的非终结符，即不含有如下形式的生成式右部： $$\cdots QR \cdots$$ ，则我们称该文法为**算符文法**

**约定：**

* $$a、b$$ 代表任意终结符
* $$P、Q、R$$ 代表任意非终结符
* $$\cdots$$ 代表由终结符和非终结符组成的任意序列、包括空字

**算符优先文法：**

假定 $$G$$ 是一个不含 $$\epsilon -$$ 产生式的算符文法。对于任何一对终结符$$a、b$$ ，我们定义：

* $$a \eqcirc b$$ 当且仅当文法 $$G$$ 中含有形如 $$P \rightarrow \dots ab \dots $$ 或 $$P \rightarrow \dots aQb \dots$$ 的产生式
* $$a \lessdot b$$ 当且仅当文法 $$G$$ 中含有形如 $$P \rightarrow \dots aR \dots $$ 的产生式，且 $$R \stackrel{+}\Rightarrow b \dots$$ 或 $$R \stackrel{+}\Rightarrow Qb \dots$$ 
* $$a \gtrdot b$$ 当且仅当文法 $$G$$ 中含有形如 $$P \rightarrow \dots Rb \dots $$ 的产生式，且 $$R \stackrel{+}\Rightarrow  \dots a$$ 或 $$R \stackrel{+}\Rightarrow aQ \dots$$

如果一个算符文法 $$G$$ 中任何终结符对（ $$a 、 b$$ ）至多只满足下述三关系之一： $$a \eqcirc b，a \lessdot b，a \gtrdot b$$ 则称 $$G$$ 是一个**算符优先文法**

### **5.4 构造优先关系表算法**

1. 通过检查 $$G$$ 的每个产生式的每个候选式，可找出所有满足 $$a \eqcirc b$$ 的终结符对
2. 确定满足关系 $$a \lessdot b，a \gtrdot b$$ 的所有终结符对
   * 首先需要对 $$G$$ 的每个非终结符 $$P$$ 构造两个集合 $$FIRSTVT(P)、LASTVT(P)$$ 
   * $$FIRSTVT(P) = \{ a \mid P \stackrel{+}\Rightarrow a \dots，或 P \stackrel{+}\Rightarrow Qa \dots，a \in V_T,Q \in V_N \}$$ 
   * $$LASTVT(P) = \{ a \mid P \stackrel{+}\Rightarrow \dots a ，或 P \stackrel{+}\Rightarrow\dots aQ ，a \in V_T,Q \in V_N \} $$ 
3. 有了这两个集合后，就可以通过检查每个产生式的候选式确定满足关系 $$a \lessdot b，a \gtrdot b$$ 的所有终结符对
   * 假定有个产生式的一个候选形为 $$ \dots aP \dots $$ ，那么，对任何  $$b \in FIRSTVT(P)$$ ，有 $$a \lessdot b$$ 
   * 假定有个产生式的一个候选形为 $$ \dots Pb \dots $$ ，那么，对任何  $$a \in LASTVT(P)$$ ，有 $$a \gtrdot b$$ 

### 5.5 构造集合 $$FIRSTVT(P)$$ 的算法

反复使用下面两条规则构造集合

1. 若有产生式 $$P \rightarrow a \dots$$ 或 $$P \rightarrow Qa \dots$$ ，则 $$a \in FTRSTVT(P)$$ 
2. 若 $$a \in FTRSTVT(Q)$$ ，且有产生式 $$P \rightarrow Q \dots$$ ，则 $$a \in FIRSTVT(P)$$ 

**数据结构**

* 布尔数组 $$F[P,a]$$ ，使得 $$F[P,a]$$ 为真的条件是，当且仅当 $$a \in FTRSTVT(P)$$ 。开始时，按上述规则（1）对每个数组元素 $$F[P,a]$$ 赋初始值
* 栈 STACK ，把所有初值为真的数组元素 $$F[P,a]$$ 的符号对 （P，a）全部放在 STACK 之中

**运算**

* 如果栈 STACK 不空，就将栈顶逐出，记此项为 （Q，a）。对于每个形如 $$P  \rightarrow Q\dots $$ 的产生式，若 $$F[P,a]$$ 为假，则变其值为真且将（P，a）推进 STACK 栈
* 上述过程必须一直重复，直至栈 STACK 拆空为止

**程序**

* > BEGIN
  >
  >         FOR 每个非终结符 P 和终结符 a DO
  >
  >                 $$F[P，a] := FALSE;$$ 
  >
  >         FOR 每个形如 $$P \rightarrow a \dots 或 P \rightarrow Qa \dots$$ 的产生式 DO
  >
  >                 INSERT\(P，a\);
  >
  >         WHILE STACK 非空 DO
  >
  >         BEGIN
  >
  >                 把 STACK 的栈顶，记为（Q，a），上托出去;
  >
  >                 FOR 每条形如 $$P \rightarrow Q \dots$$ 的产生式 DO
  >
  >                         INSERT\(P，a\);
  >
  >         END OF WHILE
  >
  > END

### 5.6 构造集合 $$LASTVT(P)$$ 的算法

反复使用下面两条规则构造集合

1. 若有产生式 $$P \rightarrow  \dots a$$ 或 $$P \rightarrow  \dots Qa$$ ，则 $$a \in LASTVT(P)$$ 
2. 若 $$a \in LASTVT(Q)$$ ，且有产生式 $$P \rightarrow \dots Q$$ ，则 $$a \in LASTVT(P)$$ 

### 5.7 构造集合示例

文法 $$G(E)$$ ：

$$E \rightarrow E + T \mid T \\  T \rightarrow T*F \mid F \\  F \rightarrow P \uparrow F \mid P \\ P \rightarrow (E) \mid i$$ 

**FIRSTVT  集合：**

|  | $$+$$  | $$*$$  | $$\uparrow$$  | $$($$  | $$)$$  | $$i$$  |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| $$E$$  | $$\surd$$  | $$\surd$$  | $$\surd$$  | $$\surd$$  |  | $$\surd$$  |
| $$T$$  |  | $$\surd$$  | $$\surd$$  | $$\surd$$  |  | $$\surd$$  |
| $$F$$  |  |  | $$\surd$$  | $$\surd$$  |  | $$\surd$$  |
| $$P$$  |  |  |  | $$\surd$$  |  | $$\surd$$  |

**LASTVT  集合：**

|  | $$+$$  | $$*$$  | $$\uparrow$$  | $$($$  | $$)$$  | $$i$$  |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| $$E$$  | $$\surd$$  | $$\surd$$  | $$\surd$$  |  | $$\surd$$  | $$\surd$$  |
| $$T$$  |  | $$\surd$$  | $$\surd$$  |  | $$\surd$$  | $$\surd$$  |
| $$F$$  |  |  | $$\surd$$  |  | $$\surd$$  | $$\surd$$  |
| $$P$$  |  |  |  |  | $$\surd$$  | $$\surd$$  |

### 5.8 构造优先关系表算法程序实现

概念请看 **5.4** 部分

* > FOR 每条产生式 $$P \rightarrow X_1 X_2 \dots X_n$$ DO
  >
  >         FOR i:= 1 TO n-1 DO
  >
  >         BEGIN
  >
  >                 IF $$X_i$$ 和 $$X_{i+1}$$ 均为终结符 THEN 置 $$X_i \eqcirc X_{i+1}$$ 
  >
  >                 IF $$i \le n-2$$ 且 $$X_i$$ 和 $$X_{i+2}$$ 均为终结符，但 $$X_{i+1}$$ 为非终结符 THEN $$X_i \eqcirc X_{i+2}$$                                                   
  >
  >                 IF $$X_i$$ 为终结符而 $$X_{i+1}$$ 为非终结符 THEN
  >
  >                         FOR $$FIRSTVT(X_{i+1})$$ 中每个 $$a$$ DO 置 $$X_i \lessdot a$$ 
  >
  >                 IF $$X_i$$ 为f非终结符而 $$X_{i+1}$$ 为终结符 THEN
  >
  >                         FOR $$LASTVT(X_i)$$ 中每个 $$a$$ DO 置 $$a \gtrdot X_{i+1}$$ 
  >
  >         END

### 5.9 示例

接着 **5.7** 的示例

$$G$$ **的算符优先关系表**

|  | $$+$$ **** | $$*$$ **** | $$\uparrow$$ **** | $$($$ **** | $$)$$ **** | $$i$$ **** |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| $$+$$ **** | $$\gtrdot$$  | $$\lessdot$$  | $$\lessdot$$  | $$\lessdot$$  | $$\gtrdot$$  | $$\lessdot$$  |
| $$*$$ **** | $$\gtrdot$$  | $$\gtrdot$$  | $$\lessdot$$  | $$\lessdot$$  | $$\gtrdot$$  | $$\lessdot$$  |
| $$\uparrow$$ **** | $$\gtrdot$$  | $$\gtrdot$$  | $$\lessdot$$  | $$\lessdot$$  | $$\gtrdot$$  | $$\lessdot$$  |
| $$($$ **** | $$\lessdot$$  | $$\lessdot$$  | $$\lessdot$$  | $$\lessdot$$  | $$\eqcirc$$  | $$\lessdot$$  |
| $$)$$ **** | $$\gtrdot$$  | $$\gtrdot$$  | $$\gtrdot$$  |  | $$\gtrdot$$  |  |
| $$i$$ **** | $$\gtrdot$$  | $$\gtrdot$$  | $$\gtrdot$$  |  | $$\gtrdot$$  |  |

### 5.10 算符优先分析算法实现

**素短语：**如果一个短语中，至少包含一个终结符，并且，除了它自身外不再含有任何更小的素短语

**最左素短语：**处于句型最左边的那个素短语

**例子：**文法 ****$$G(E)$$ 

                    ****$$E \rightarrow E + T \mid T \\  T \rightarrow  T * F \mid F \\ F \rightarrow P \uparrow F \mid P \\  F \rightarrow (E) \mid i$$                                        ****![](../.gitbook/assets/image%20%2869%29.png) ****

短语： $$T、F、P、i、F*P、T+F*P、T+F*P+i$$ 

直接短语： $$T、F、P、i$$ 

句柄： $$T$$ 

素短语： $$F*P、i$$ 

最左素短语： $$F*P$$ 



上面的例子是人工手动找最左素短语，下面介绍编程自己找最左素短语



算法优先文法句型（包括两个 \# 之间）的一般形式写成：$$\# N_1 a_1 N_2 a_2 \dots N_n a_n N_{n+1} \#$$ ，其中每个 $$a_i$$ 都是终结符， $$N_i$$ 是可有可无的非终结符



**定理：**一个算符优先文法 $$G$$ 的任何句型的最左素短语是满足如下条件的最左子串



