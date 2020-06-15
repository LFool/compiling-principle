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



**定理：**一个算符优先文法 $$G$$ 的任何句型的最左素短语是满足如下条件的最左子串 $$N_j a_j \dots N_i a_i N_{i+1} $$ 

* $$a_{j-1} \lessdot a_j$$ 
* $$a_j \eqcirc a_{j+1}, \dots , a_{i-1} \eqcirc a_i$$ 
* $$a_i \gtrdot a_{i+1}$$ 

**算法：**

* > k := 1;
  >
  > S\[k\] := '\#';
  >
  > REPEAT
  >
  >         把下一个输入符号读进 a 中;
  >
  >         IF S\[k\] $$\in V_T$$ THEN j := k ELSE j := k - 1;
  >
  >         WHILE S\[j\] $$\gtrdot$$ a DO
  >
  >         BEGIN
  >
  >                 REPEAT
  >
  >                         Q := S\[j\];
  >
  >                         IF S\[j - 1\] $$\in V_T$$ THEN j := j - 1 ELSE j := j - 2;
  >
  >                 UNTIL S\[j\] $$\lessdot Q$$ 
  >
  >                 把 S\[j + 1\] ... S\[k\] 归约为某个 N;
  >
  >                 k := j + 1;
  >
  >                 S\[k\] := N;
  >
  >         END OF WHILE; 
  >
  >         IF S\[j\] $$\lessdot$$ a OR S\[j\] $$\eqcirc$$ a THEN
  >
  >                 BEGIN k := k + 1; S\[k\] := a END
  >
  >         ELSE ERROR /\* 调用出错诊断程序 \*/
  >
  > UNTIL a='\#'

### 5.11 优先函数

每个终结符 $$\theta$$ 与两个自然数 $$f(\theta) $$ 与 $$g(\theta) $$ 相对应，使得：

* 若 $$\theta_1 \lessdot \theta_2$$ ，则  $$f(\theta_1) < g(\theta_2) $$ 
* 若 $$\theta_1 \eqcirc \theta_2$$ ，则  $$f(\theta_1) = g(\theta_2) $$ 
* 若 $$\theta_1 \gtrdot \theta_2$$ ，则  $$f(\theta_1) > g(\theta_2) $$ 

优点：便于比较、节省空间

缺点：原本不需要比较优先关系的两个终结符，由于自然数的对应，变得可以比较，需要进行一些判断

**构造优先函数**

1. 画图：对于每个终结符 $$a$$ ，令其对应两个符号 $$f_a$$ 和 $$g_a$$ ，画一条所有符号和为结点的方向图。如果 $$a \gtrdot \eqcirc b$$ ，则从 $$f_a$$ 画一条弧至 $$g_a$$ ；如果 $$a \lessdot \eqcirc b$$ ，则从 $$g_a$$ 画一条弧至 $$f_a$$ 
2. 数数：对每个结点都赋予一个数，次数等于从该结点出发所能到达的结点（包括出发点自身）。赋予 $$f_a$$ 的数作为 $$f(a)$$ ，赋予 $$g_a$$ 的数作为 $$g(a)$$ 
3. 验证：检查所构造出来的函数 $$f$$ 和 $$g$$ 是否与原来的关系矛盾。若没有矛盾，则 $$f$$ 和 $$g$$ 就是要求的优先函数；若有矛盾，则不存在优先函数

**注意：**

1. 优先函数不唯一
2. 就算构造出来了优先函数，也可能存在不矛盾，所以必须检查优先函数是否满足要求

## 6. LR 分析法

上述算符优先分析算法采用素短语来实现栈顶归约；而 LR 分析法只用利用句柄来进行归约

**LR 分析法的思路：**

* 产生分析表

  文法 $$\longrightarrow$$ 分析表产生器 $$\longrightarrow$$ 分析表

* LR 分析器工作

  输入 $$\longrightarrow$$ LR 分析总控程序（根据**分析表**） $$\longrightarrow$$ 输出

### 6.1 总控程序（LR 分析器）的工作原理

实际上 LR 分析法是一种**规范归约**，关键问题在于**寻找句柄**

* **历史：**已移入符号栈的内容
* **展望：**根据产生式推测未来可能遇到的输入符号
* **实现：**当前的输入符号

LR 分析法：把“历史”及“展望”综合抽象成**状态**；由栈顶的**状态**和**现行的输入符号**唯一确定每一步的工作

![](../.gitbook/assets/image%20%2876%29.png)

LR 分析器的核心就是一张分析表

* $$ACTION[s, a]$$ ：当状态 s 面临输入符号 a 时，应采取什么动作
* $$GOTO[s, X]$$ ：状态 s 面对文法符号 X 时，下一状态是什么
* 移进（shift）：把（s，a）的下一个状态 s' 和 输入符号 a 推进栈，下一符号变为现行输入符号
* 归约（reduce）：用某产生式 $$A \rightarrow \beta$$ 进行归约，假若 $$\beta$$ 的长度为 r，归约作为是，去除栈顶 r 个项，使状态 $$s_{m-r}$$ 变为栈顶，然后把 $$(s_{m-r}, A)$$ 的下一状态 $$s' = GOTO[s_{m-r}, A]$$ 和文法符号 A 推进栈



        状态                   已归约串                  输入串

$$(s_0s_1 \dots s_m，\# X_1 \dots X_m， a_ia_{i+1} \dots a_n \#)$$ 

分析器根据 $$ACTION[s_m, a_i]$$ 确定下一动作

* 若 $$ACTION[s_m, a_i]$$ 为移进，且 s 为下一状态，则三元式格局变为： $$(s_0s_1 \dots s_ms，\# X_1 \dots X_ma_i， a_{i+1} \dots a_n \#)$$ 
* 若 $$ACTION[s_m, a_i]$$ 为按 $$A \rightarrow \beta$$ 归约 ，三元式变为： $$(s_0s_1 \dots s_{m-r}s，\# X_1 \dots X_{m-r}A， a_ia_{i+1} \dots a_n \#)$$ 

  此处， $$s=GOTO(s_{m-r},A)$$ ，r 为 $$\beta$$ 的长度， $$\beta = X_{m-r+1}\dots X_m$$ 

* 若 $$ACTION[s_m, a_i]$$ 为“接受”，则三元式不再变化，变化过程终止，宣布分析成功
* 若 $$ACTION[s_m, a_i]$$ 为“报错”，则三元式变化过程终止，报告错误

#### 

#### 示例：

文法 G\(E\)：

$$1. E \rightarrow E + T \\  2. E \rightarrow T \\  3. T \rightarrow T * F \\  4. T \rightarrow F \\ 5. F \rightarrow (E) \\ 6. F \rightarrow i$$   ![](../.gitbook/assets/image%20%2875%29.png) 

| 步骤 | 状态 | 符号 | 输入串 |
| :---: | :---: | :---: | :---: |
| 1 | 0 | $$\#$$  | $$i*i+i\#$$  |
| 2 | 0 5 | $$\# i$$  | $$*i+i\#$$  |
| 3 | 0 3 | $$\# F$$  | $$*i+i\#$$  |
| 4 | 0 2 | $$\# T$$  | $$*i+i\#$$  |
| 5 | 0 2 7 | $$\# T*$$  | $$i+i\#$$  |
| 6 | 0 2 7 5 | $$\# T*i$$  | $$+i\#$$  |
| 7 | 0 2 7 10 | $$\# T*F$$  | $$+i\#$$  |
| 8 | 0 2 | $$\# T$$  | $$+i\#$$  |
| 9 | 0 1 | $$\# E$$  | $$+i\#$$  |
| 10 | 0 1 6 | $$\# E +$$  | $$i\#$$  |
| 11 | 0 1 6 5 | $$\# E + i$$  | $$\#$$  |
| 12 | 0 1 6 3 | $$\# E + F$$  | $$\#$$  |
| 13 | 0 1 6 9 | $$\# E + T$$  | $$\#$$  |
| 14 | 0 1 | $$\# E$$  | $$\#$$  |
| 15 | 接受 |  |  |

### 6.2 LR 文法

* 对于一个文法，如果能够构造一张分析表，使得它的每个入口均是唯一确定的，则这个文法就称为 **LR 文法**
* 一个文法，如果能用一个每步顶多向前检查 k 个输入符号的 LR 分析器进行分析，则这个文法就称为 **LR\(k\) 文法**

### 6.3 LR\(0\) 项目集规范族的构造

**字的前缀**：指字的任意首部，如字 $$abc$$ 的前缀有 $$\epsilon , a, ab, abc$$ 

**活前缀**：不含句柄之后的任何符号的前缀。对于规范句型 $$\alpha\beta\gamma$$ ， $$\beta$$ 为句柄，如果 $$\alpha \beta = u_1u_2 \dots u_r$$ ，则符号串 $$u_1u_2\dots u_i (1 \le i \le r)$$ 是 $$\alpha\beta\gamma$$ 的活前缀（ $$\gamma $$ 必定为终结符串）

规范归约的过程中，保证分析栈中的总是**活前缀**，就说明分析采用的移进 / 归约动作是正确的



项目概念：文法 G 的每个产生式的右部添加一个圆点称为 G 的 **LR\(0\) 项目**

如： $$A \rightarrow XYZ$$ 有四个项目： $$A \rightarrow \cdot XYZ；A \rightarrow  X\cdot YZ；A \rightarrow  XY\cdot Z；A \rightarrow  XYZ\cdot $$ 

* $$A \rightarrow \alpha \cdot$$ 称为 “**归约项目**”
* 归约项目 $$S'\rightarrow \alpha \cdot$$ 称为 “**接受项目**”
* $$A \rightarrow \alpha \cdot a \beta (a \in V_T) $$ 称为 “**移进项目**”
* $$A \rightarrow \alpha \cdot B \beta (B \in V_N) $$ 称为 “**待约项目**”

构造识别文法所有活前缀的 NFA 方法

1. 若状态 i 为 $$X \rightarrow X_1 \cdots X_{i+1} \cdot X_i \cdots X_n$$ ，状态 j 为 $$X \rightarrow X_1 \cdots X_{i-1} X_i \cdot X_{i+1} \cdots X_n$$ ，则从状态 i 画一条标志为 $$X_i$$ 的有向边到状态 j
2. 若状态 i 为 $$X \rightarrow \alpha \cdot A \beta$$ ，A 为非终结符，则从状态 i 画一条 $$\epsilon $$ 边到所有状态 $$A \rightarrow \cdot \gamma$$ 



**例：**文法 $$G(S')$$ 

$$S' \rightarrow E \\ E \rightarrow aA \mid bB \\ A \rightarrow cA \mid d \\ B \rightarrow cB \mid d$$ 

该文法的项目有：

$$1. S' \rightarrow \cdot E \\ 2. S' \rightarrow  E \cdot $$  $$3. E \rightarrow \cdot aA \\ 4. E \rightarrow a \cdot A \\ 5. E \rightarrow a A \cdot$$  $$6. E \rightarrow \cdot bB \\  7. E \rightarrow b \cdot B \\   8. E \rightarrow b B \cdot $$   $$9. A \rightarrow \cdot cA \\  10. A \rightarrow c\cdot A \\  11. A \rightarrow cA\cdot  $$  

$$12. A \rightarrow \cdot d \\ 13. A \rightarrow d\cdot$$  $$14. B \rightarrow \cdot cB \\ 15. B \rightarrow c\cdot B \\   16. B \rightarrow cB\cdot  $$ $$17. B \rightarrow \cdot d \\ 18. B \rightarrow d\cdot$$ 

识别活前缀的 NFA

![](../.gitbook/assets/image%20%2877%29.png)

识别活前缀 DFA

![](../.gitbook/assets/image%20%2873%29.png)

识别构成一个文法活前缀的 DFA 的项目集（状态）的全体称为文法的一个 **LR\(0\) 项目集规范族**

\*\*\*\*

**有效项目**

我们说项目 $$A \rightarrow \beta_1 \cdot \beta_2$$ 对活前缀 $$\alpha \beta_1$$ 是有效的，其条件是存在规范推导 $$S' \stackrel{*} \Rightarrow \alpha A \omega \Rightarrow \alpha \beta_1 \beta_2 \omega$$ 



**拓广文法**

* 假设文法 G，以 S 为开始符号
* 构造一个G'，它包含了整个 G，但引进了一个不出现在 G 中的非终结符 S'，并加进一个新的产生式 $$S' \rightarrow S$$ ，S' 是 G' 的开始符号
* 称 G' 是 G 的**拓广文法**
* G' 唯一的接受态：仅含项目 $$S' \rightarrow S \cdot$$ 的状态



**闭包 CLOSURE**

假设 I 是文法 G' 的任一项目集，定义和构造 I 的闭包 CLOSURE\(I\) 如下：

* I 的任何项目集都属于 CLOSURE\(I\)
* 若 $$A \rightarrow \alpha \cdot B \beta$$ 属于 CLOSURE\(I\)，那么对任何关于 B 的产生式 $$B \rightarrow \gamma$$ ，项目 $$B \rightarrow \cdot \gamma$$ 也属于CLOSURE\(I\)
* 重复执行上述两个步骤直至 CLOSURE\(I\) 不再增大为止



**GO函数**

为了识别活前缀，我们定义一个状态转换函数 GO 的一个状态转换函数。I 是一个项目集，X 是一个文法符号。函数值 GO\(I, X\) 定义为： $$GO(I, X) = CLOSURE(J)$$ 其中 $$J = \{任何形如 A \rightarrow \alpha X \cdot \beta 的项目 \mid A \rightarrow \alpha \cdot X  \beta \in I \}$$ 

直观上说，若 I 是对某个活前缀 $$\gamma$$ 有效的项目集，那么，GO\(I, X\) 便是对 $$\gamma X$$ 有效的项目集

\*\*\*\*

**例：**文法 $$G(S')$$ 

$$S' \rightarrow E \\ E \rightarrow aA \mid bB \\ A \rightarrow cA \mid d \\ B \rightarrow cB \mid d$$ 

$$I_0 = \{ S' \rightarrow \cdot E, E \rightarrow \cdot aA, E \rightarrow \cdot bB \}$$ ****

* $$\begin{aligned} GO(I_0, E) = closure(J) = & closure(\{ S' \rightarrow E \cdot \}) \\ = & \{ S' \rightarrow E \cdot \} = I_1 \end{aligned}$$ 
* $$\begin{aligned}  GO(I_0, a) = closure(J) = & closure(\{ E \rightarrow a \cdot A \}) \\ = & \{ E \rightarrow  a \cdot A, A \rightarrow \cdot cA, A \rightarrow \cdot d \} = I_2 \end{aligned} $$ 
* $$\begin{aligned}  GO(I_0, b) = closure(J) = & closure(\{ E \rightarrow b \cdot B \}) \\ = & \{ E \rightarrow  b \cdot B, B \rightarrow \cdot cB, B \rightarrow \cdot d \} = I_3 \end{aligned} $$ 



通过 **6.2** 和 **6.3** 我们学了两种构造识别活前缀的 DFA 的方法

* $$项目 \rightarrow NFA \rightarrow DFA$$ 
* $$Closure \rightarrow GO \rightarrow DFA$$ 

### 6.4 LR\(0\) 分析表

**LR\(0\) 文法**

假设一个文法 G 的拓广文法 G' 的活前缀识别自动机中的每个状态（项目集）不存在下列情况：

1. 既含移进项目又含归约项目
2. 含多个归约项目

则称 G 是一个 **LR\(0\) 文法**

\*\*\*\*

#### **构造 LR\(0\) 分析表的算法**

令每个项目集 $$I_k$$ 的下标 k 作为分析器的状态，包含项目 $$S' \rightarrow \cdot S$$ 的集合 $$I_k$$ 的下标 k 的分析器的初态



LR\(0\)分析表的 ACTION 和 GOTO 子表构造

1. 若项目 $$A \rightarrow \alpha \cdot a \beta$$ 属于 $$I_k$$ 且 $$GO(I_k, a) = I_j$$ ， $$a$$ 为终结符，则置 $$ACTION[k, a]$$ 为 $$sj$$ 
2. 若项目 $$A \rightarrow \alpha \cdot$$ 属于 $$I_k$$ ，那么，对**任何终结符** $$a$$ （或结束符 \#），置 $$ACTION[k, a]$$ 为 $$rj$$ （假定产生式 $$A \rightarrow \alpha$$ 是文法 G' 的第 j 个产生式）
3. 若项目 $$S' \rightarrow S \cdot$$ 属于 $$I_k$$ ，则置 $$ACTION[k, \#]$$ 为 $$acc$$ 
4. 若 $$GO(I_k, A) = I_j$$ ，A 为非终结符，则置 $$GOTO[k, A] = j$$ 
5. 分析表中凡不能用规则 1 至 4 填入信息的空白格均置入“报错信息”



**接着 6.3 中的例子，构造分析表**

![](../.gitbook/assets/image%20%2871%29.png)

### **6.5 SLR 分析表**

LR\(0\) 文法太简单，没有实用价值。一个项目集里可能既存在移进项目，也存在归约项目，会产生冲突



**消除冲突：引入 FOLLOW 集**

假定一个 LR\(0\) 规范族中含有如下的一个项目集（状态） $$I = \{ X \rightarrow \alpha \cdot b \beta , A \rightarrow \alpha \cdot , B \rightarrow \alpha\cdot \}$$ 。 $$FOLLOW(A)$$ 和 $$FOLLOW(B)$$ 的交集为 $$\phi$$ ，且不包含 $$b$$ ，那么，当状态 $$I$$ 面临任何输入符号 $$a$$ 时，可以：

1. 若 $$a = b$$ ，则移进
2. 若 $$a \in FOLLOW(A)$$ ，用产生式 $$A \rightarrow \alpha $$ 进行归约
3. 若 $$a \in FOLLOW(B)$$ ，用产生式 $$B \rightarrow \alpha $$ 进行归约
4. 此外，报错



一般情况：

假定 LR\(0\) 规范族的一个项目集 $$\begin{aligned} I = \{  & A_1 \rightarrow \alpha \cdot a_1 \beta_1, A_2 \rightarrow \alpha \cdot a_2 \beta_2 ,  \cdots ,  A_m \rightarrow \alpha \cdot a_m \beta_m,  \\  & B_1 \rightarrow \alpha \cdot , B_2 \rightarrow \alpha \cdot , \cdots , B_n \rightarrow \alpha \cdot ,  \} \end{aligned}$$ 

如果集合 $$\{ a_1, \cdots, a_m \} ， FOLLOW(B_1), \cdots , FOLLOW(B_n)$$ 两两不相交（包括不得有两个 FOLLOW 集合有 \#），则：

1. 若 $$a $$ 是某个 $$a_i，i = 1,2,\cdots , m$$ ，则移进
2. 若 $$a \in FOLLOW(B_i)， i = 1,2,\cdots , n$$ ，则用产生式 $$B_i \rightarrow \alpha$$ 进行归约
3. 此外，报错

### 6.6 构造 SLR\(1\) 分析表方法

首先把 G 拓广为 G'，对 G' 构造 LR\(0\) 项目集规范族 C 和活前缀识别自动机的状态 转换函数 GO

然后使用 C 和 GO ，按照下面的算法构造 SLR 分析表

* 令每个项目集 $$I_k$$ 的下标 k 作为分析器的状态，包含项目 $$S' \rightarrow \cdot S$$ 的集合 $$I_k$$ 的下标 k 的分析器的初态



SLR\(1\)分析表的 ACTION 和 GOTO 子表构造

1. 若项目 $$A \rightarrow \alpha \cdot a \beta$$ 属于 $$I_k$$ 且 $$GO(I_k, a) = I_j$$ ， $$a$$ 为终结符，则置 $$ACTION[k, a]$$ 为 $$sj$$ 
2. 若项目 $$A \rightarrow \alpha \cdot$$ 属于 $$I_k$$ ，那么，对**任何终结符** $$a$$ ， $$a \in FOLLOW(A)$$ ，置 $$ACTION[k, a]$$ 为 $$rj$$ （假定产生式 $$A \rightarrow \alpha$$ 是文法 G' 的第 j 个产生式）
3. 若项目 $$S' \rightarrow S \cdot$$ 属于 $$I_k$$ ，则置 $$ACTION[k, \#]$$ 为 $$acc$$ 
4. 若 $$GO(I_k, A) = I_j$$ ，A 为非终结符，则置 $$GOTO[k, A] = j$$ 
5. 分析表中凡不能用规则 1 至 4 填入信息的空白格均置入“报错信息”



SLR\(1\) 和 LR\(0\) 构造的区别：**只有第二步不同**

\*\*\*\*

**接着 6.3 中的例子，构造分析表**

![](../.gitbook/assets/image%20%2872%29.png)

### \*\*\*\*

### **6.7 LR\(1\) 分析表的构造**

\*\*\*\*

**SLR冲突消解存在的问题：**计算 FOLLOW 集合所得到的超前符号集合可能大于实际能出现的超前符号集

* SLR 在方法中，如果项目集 $$I_i$$ 含项目 $$A \rightarrow \alpha \cdot$$ ，而且下一个输入符号 $$a \in FOLLOW(A)$$ ，则状态 i 面临 $$a$$ 时，可选用 “用 $$A \rightarrow \alpha$$ 归约” 动作
* 但在有些情况下，当状态 i 显示在栈顶时，栈里的活前缀未必允许把 $$\alpha$$ 归约成 A，因为可能根本就不存在一个形如 $$\beta A \alpha$$ 的规范句型
* 在这种情况下，用 $$A \rightarrow \alpha$$ 归约不一定合适
* **FOLLOW 集合提供的信息太宽泛**

\*\*\*\*

我们需要重新定义项目，使得每个项目都附带 k 个终结符

* 每个项目的一般形式是 $$[A \rightarrow \alpha \cdot \beta ，a_1a_2 \cdots a_k]$$ ，这样的一个项目称为一个 LR\(k\) 项目。项目中的 $$a_1a_2\cdots a_k$$ 称为它的**向前搜索符串** （**展望串**）
* 向前搜索符串 仅对 归约项目 $$[A \rightarrow \alpha \cdot ，a_1a_2 \cdots a_k]$$ 有意义
* 对于任何移进或待约项目 $$[A \rightarrow \alpha \cdot \beta ，a_1a_2 \cdots a_k] , \beta \ne \epsilon$$ ，搜索符串 $$a_1a_2 \cdots a_k$$ 没有直接作用



我们研究当 $$k = 1$$ 的情况



**有效项目**

形式上我们说一个 LR\(1\)项目 $$[A \rightarrow \alpha \cdot \beta, a]$$ 对活前缀 $$\gamma$$ 是有效的，如果存在规范推导 $$S' \stackrel{*} \Rightarrow \delta A \omega \Rightarrow \delta \alpha \beta \omega$$

* 其中 $$\gamma = \delta \alpha$$ 
* $$a$$ 是 $$\omega$$ 的第一个符号，或者 $$a$$ 为 \# 而 $$\omega$$ 为 $$\epsilon$$ 



**LR\(1\) 项目集规范族**

需要两个函数 CLOSURE 和 GO

$$[A \rightarrow \alpha \cdot B \beta , a]$$ 对活前缀 $$\gamma = \delta \alpha$$ 是有效的，则对于每个形如 $$B = \xi$$ 的产生式，对任何 $$b \in FIRST(\beta a)，[B \rightarrow \cdot \xi, b]$$ 对 $$\gamma$$ 也是有效的



**项目集的闭包 CLOSURE\(I\)**

1. I 的任何项目都属于 CLOSURE\(I\)
2. 若项目 $$[A \rightarrow \alpha \cdot B \beta , a]$$ 属于 CLOSURE\(I\)， $$B = \xi$$ 是一个产生式，那么，对于 $$FIRST(\beta a) $$ 的每个终结符 b，如果 $$[B \rightarrow \cdot \xi, b]$$ 原来不在 CLOSURE\(I\) 中，则把它加进去
3. 重复步骤 2 ，直至 LCLOSURE\(I\) 不再增大为止



**项目集的装换函数 GO**

令I 是一个项目集，X 是一个文法符号，函数 GO\(I, X\) 定义为： $$GO(I, X) = CLOSURE(J)$$ 其中 $$J = \{任何形如 [A \rightarrow \alpha X \cdot \beta, a] 的项目 \mid [A \rightarrow \alpha \cdot X  \beta, a] \in I \}$$ 



#### **构造 LR\(1\) 分析表的算法**

令每个项目集 $$I_k$$ 的下标 k 作为分析器的状态，包含项目 $$[S' \rightarrow \cdot S, \# ]$$ 的集合 $$I_k$$ 的下标 k 的分析器的初态



LR\(0\)分析表的 ACTION 和 GOTO 子表构造

1. 若项目 $$[A \rightarrow \alpha \cdot a \beta, b]$$ 属于 $$I_k$$ 且 $$GO(I_k, a) = I_j$$ ， $$a$$ 为终结符，则置 $$ACTION[k, a]$$ 为 $$sj$$ 
2. 若项目 $$[A \rightarrow \alpha \cdot, a]$$ 属于 $$I_k$$ ，则置 $$ACTION[k, a]$$ 为 $$rj$$ （假定产生式 $$A \rightarrow \alpha$$ 是文法 G' 的第 j 个产生式）
3. 若项目 $$[S' \rightarrow \cdot S, \# ]$$ 属于 $$I_k$$ ，则置 $$ACTION[k, \#]$$ 为 $$acc$$ 
4. 若 $$GO(I_k, A) = I_j$$ ，A 为非终结符，则置 $$GOTO[k, A] = j$$ 
5. 分析表中凡不能用规则 1 至 4 填入信息的空白格均置入“报错信息”



**示例：**拓广文法 G\(S'\)

$$0. S' \rightarrow S \\ 1. S \rightarrow BB \\ 2. B \rightarrow aB \\ 3. B \rightarrow b$$ 

![](../.gitbook/assets/image%20%2881%29.png)

![](../.gitbook/assets/image%20%2878%29.png)









\*\*\*\*

\*\*\*\*

\*\*\*\*

\*\*\*\*



