# 第五章 语义分析

## 1 属性文法

### **1.1 属性文法**

在上下文无关文法的基础上，为每个文法符号（终结符或非终结符）配备若干相关的 “值”（称为**属性**）

* **属性**代表与文法相关信息，如类型、值、代码序列、符号表内容等
* 属性可以进行计算和传递
* **语义规则**：对于文法的每个产生式都配备了一组属性的计算规则

### **1.2 属性**

* **综合属性**：“自下而上” 传递信息
* **继承属性**：“自上而下” 传递信息

在一个属性文法中，对应于每个产生式 $$A \rightarrow \alpha$$ 都有一套与之相关的**语义规则**，每条规则的形式为： $$b := f(c_1, c_2, \cdots, c_k)$$ ，这里， $$f$$ 是一个函数，而且或者：

1. b 是 A 的一个**综合属性** 并且  $$c_1, c_2, \cdots, c_k$$ 是产生式右边文法符号的属性
2. b 是产生式右边某个文法符号的一个**继承属性** 并且 $$c_1, c_2, \cdots, c_k$$ 是 A 或产生式右边任何文法符号的属性

### **1.3 说明**

* 终结符只有**综合属性**，由词法分析器提供
  * $$F \rightarrow digit$$ 
  * $$digit.lexval$$ 
* 非终结符既可有**综合属性**也可用**继承属性**，文法开始符号的所有继承属性作为属性计算前的初始值
  * $$F \rightarrow digit$$ 
  * $$F.val、digit.lexval$$ 
* 对于出现**产生式右边的继承属性**和**出现产生式左边的综合属性**都必须提供一个计算规则。属性计算规则只能使用相应产生式中的文法符号属性
  * $$F \rightarrow digit$$ 
  * $$F.val := digit.lexval$$ 
* 出现在**产生式左边的继承属性**和**出现在产生式右边的综合属性**不得由所给的产生式的属性计算规则计算，由其它产生式的属性规则计算或者由属性计算器的参数提供
* 语义规则所描述的工作可以包括属性计算、静态语义检查、符号表操作、代码生成等等

### 1.4 例子

非终结符考虑 A、B 和 C，其中：

* A 有一个继承属性 a 和一个综合属性 b；
* B 有综合属性 c；
* C 有继承属性 d；

产生式 $$A \rightarrow BC$$ 可能的规则有： $$C.d := B.c + 1 \\  A.b := A.a + B.c $$ ，而属性 A.a 和 B.c 在其他地方计算

### 1.5 总结

综合属性：只能由 等式右边 的 **子结点** 和 **本身** 计算

继承属性：只能由 等式左边 的 **父节点** 、**兄弟结点** 和 **本身** 计算

## 2. 基于属性文法的处理方法

输入串 $$\longrightarrow $$ 语法树 $$\longrightarrow $$ 按照语义规则计算属性

由源程序的语法结构所驱动的处理办法就是**语法制导翻译法**

**语义规则的计算**

* 产生代码
* 在符号表中存放信息
* 给出错误信息
* 执行任何其他动作

对输入符号串的**翻译**也就是根据语义规则进行**计算**的结果

### 2.1 依赖图

在一颗语法树中的结点的继承属性和综合属性之间的相互依赖关系可以由依赖图（有向图）来描述

为每一个包含过程调用的语义规则引入一个虚综合属性 b，这样把每一个语义规则都写成 $$b := f(c_1, c_2, \cdots, c_k)$$ 的形式

依赖图中为每一个属性设置一个结点，如果属性 b 依赖于属性 c，则从属性 c 的结点有一条有向边连到属性 b 的结点

**如：**

![](.gitbook/assets/image%20%2886%29.png)

**例子：**句子 $$real、id_1、id_2、id_3$$ 的带注释的语法树的依赖图

| 产生式 | 语义规则 | 添加边 |
| :---: | :---: | :---: |
| $$D \rightarrow TL$$  | $$L.in := T.type$$  | T 指向 L |
| $$T \rightarrow int$$  | $$T.type := integer$$  |  |
| $$T \rightarrow real$$  | $$T.type := real$$ |  |
| $$L \rightarrow L_1, id$$  | $$L_1.in := L.in \\ addtype(id.netry, L.in)$$ | $$L 指向 L_1 \\ L 和 id 指向 addtype$$  |
| $$L \rightarrow id$$  | $$addtype(id.netry, L.in)$$ | $$L 和 id 指向 addtype$$  |

![](.gitbook/assets/image%20%2882%29.png)

**良定义的属性文法**

* 如果一属性文法不存在属性之间的循环依赖关系，那么称该文法为良定义的

**属性的计算次序**

* 通过图的**拓扑排序**来确定属性的计算次序

输入串 $$\longrightarrow $$ 语法树 $$\longrightarrow $$ 依赖图 $$\longrightarrow $$ 语义规则计算次序



上面的例子的属性计算次序是：

 $$a_4 := real; \\  a_5 := a_4; \\  addtype(id_3.entry, a_5); \\  a_7 := a_5; \\  addtype(id_3.entry, a_7); \\  a_9 := a_7; \\  addtype(id_3.entry, a_9); $$ 

### 2.2 树遍历

通过树遍历的方法计算属性的值

* 假设语法树已建立，且树中已带有开始符号的继承属性和终结符的综合属性
* 以某种次序遍历语法，直至计算出所有属性
  * 深度优先，从左到右的遍历
* > while 还有未被计算的属性 do
  >
  >         visitNode\(S\) /\* S 是开始符号 \*/
  >
  > procedure visitNode\(N:Node\);
  >
  > begin
  >
  >         if N 是一个非终结符 then
  >
  >                 /\* 假设它的产生式为 $$N \rightarrow X_1 \cdots X_m$$ \*/
  >
  >                 for i := i to m do
  >
  >                         if $$X_i \in V_N$$ then /\* 即 $$X_i$$ 是非终结符 \*/
  >
  >                         begin
  >
  >                                 计算 $$X_i$$ 的所有能够计算的继承属性;
  >
  >                                 visitNode\( $$X_i$$ \)
  >
  >                         end;
  >
  >                 计算 N 的所有能够计算的综合属性
  >
  > end

### 2.3 一边扫描

* 一遍扫描的处理方法是在语法分析的同时计算属性值
* **L - 属性文法** 适合于一遍扫描的自上而下分析
* **S - 属性文法** 适合于一遍扫描的自下而上分析



**语法制导翻译法**

直观上就是为文法中每个产生式配上一组语义规则，并且在语法分析的同时执行这些语义规则

按语义规则被计算的时机分为：

* 在自上而下语法分析中，一个产生式匹配输入串成功时
* 在自下而上语法分析中，一个产生式被用于进行归约时



**抽象语法树**

在语法树中去掉那些对翻译不必要的信息，从而获取更有效的源程序中间表达。这种经变换后的语法树称之为**抽象语法树**

![](.gitbook/assets/image%20%2878%29.png)

\*\*\*\*

**简历表达式的抽象语法树**

* $$mknode(op, left, right)$$ 建立一个运算符号结点，标号是 op，两个域指针 left 和 right 分别指向左子树和右子树
* $$mkleaf(id, entry)$$ 建立一个标识符结点，标号为 id，一个域 entry 指向标识符在符号表中的入口
* $$mkleaf(num, val)$$ 建立一个数结点，标号为 num，一个域 val 用于存放数的值

建立抽象语法树的语义规则

| 产生式 | 语义规则 |
| :--- | :--- |
| $$E \rightarrow E_1 + T$$  | $$E.nptr := mknode('+', E_1.nptr, T.nptr)$$  |
| $$E \rightarrow E_1 - T$$  | $$E.nptr := mknode('-', E_1.nptr, T.nptr)$$  |
| $$E \rightarrow T$$  | $$E.nptr := T.nptr$$  |
| $$T \rightarrow (E)$$  | $$T.nptr := E.nptr$$  |
| $$T \rightarrow id$$  | $$T.nptr := mkleaf(id, id.entry)$$  |
| $$T \rightarrow num$$  | $$T.nptr := mkleaf(num, num.val)$$  |

构造 $$a - 4 + c$$ 的抽象语法树

![](.gitbook/assets/image%20%2884%29.png)

## 3. S- 属性文法的自下而上计算

**定义**

* **S- 属性文法**：只含有综合属性
* **综合属性** 可以在分析输入符号串的同时由自下而上的分析器来计算
* 分析器可以保存与栈中文法符号有关的**综合属性**值，每当进行归约时，新的属性值就由栈中正在归约的产生式右边符号的属性值来计算



**S- 属性文法的计算**

* 在分析栈中使用一个附加的域来存放综合属性值
* 假设语义规则 $$A.a := f(X.x, Y.y, Z.z)$$ 是对应于产生式 $$A \rightarrow XYZ$$ 

![](.gitbook/assets/image%20%2880%29.png)



**例子**

| 产生式 | 语义规则 | 代码段 |
| :---: | :---: | :---: |
| $$L \rightarrow En$$  | $$print(E.val)$$  | $$print(val[top])$$  |
| $$E \rightarrow E_1 + T$$  | $$E.val := E_1.val + T.val$$  | $$val[ntop] := val[top -2] + val[top]$$  |
| $$E \rightarrow T$$  | $$E.val := T.val$$  |  |
| $$T \rightarrow T_1 * F$$  | $$T.val := T_1.val * F.val$$  | $$val[ntop] := val[top -2] * val[top]$$  |
| $$T \rightarrow F$$  | $$T.val := F.val$$  |  |
| $$F \rightarrow (E)$$  | $$F.val := E.val$$  | $$val[ntop] := val[top -1]$$  |
| $$F \rightarrow digit$$  | $$F.val := digit.lexval$$  |  |

| state | sym | val | 输入 | 用到的产生式 |
| :--- | :--- | :--- | :--- | :--- |
| 0 | $$\# $$  | $$-$$  | $$3 * 5 + 4n$$  |  |
| 0 5 | $$\# 3$$  | $$- 3$$  | $$* 5 + 4n$$  |  |
| 0 3 | $$\# F$$  | $$- 3$$  | $$* 5 + 4n$$  | $$F \rightarrow digit$$  |
| 0 2 | $$\# T$$  | $$- 3$$  | $$* 5 + 4n$$  | $$T \rightarrow F$$  |
| 0 2 7 | $$\# T *$$  | $$- 3 -$$  | $$5 + 4n$$  |  |
| 0 2 7 5 | $$\# T * 5$$  | $$- 3 - 5$$  | $$+ 4n$$  |  |
| 0 2 7 5 10 | $$\# T * F$$  | $$- 3 - 5$$  | $$+ 4n$$ | $$F \rightarrow digit$$  |
| 0 2 |  $$\# T$$ |  $$- 15$$ | $$+ 4n$$ |  $$T \rightarrow T * F$$  |
| 0 1 | $$\# E$$  | $$- 15$$  | $$+ 4n$$ | $$E \rightarrow T$$  |
| 0 1 6 | $$\# E +$$  | $$- 15 -$$  | $$4n$$ |  |
| 0 1 6 5 | $$\# E + 4$$  | $$- 15 - 4$$  | $$n$$  |  |
| 0 1 6 3 | $$\# E + F$$  | $$- 15 - 4$$  | $$n$$  | $$F \rightarrow digit$$  |
| 0 1 6 9 | $$\# E + T$$  | $$- 15 - 4$$  | $$n$$  | $$T \rightarrow F$$  |
| 0 1 | $$\# E$$  | $$- 19$$  | $$n$$  | $$E \rightarrow E + T$$  |
|  | $$\# En$$ **** | $$- 19 - $$  |  |  |
|  | $$\# L$$  | $$- 19$$  |  | $$L \rightarrow En$$  |

## 4. L- 属性文法的自顶向下翻译

**自顶向下翻译**

* 通过深度优先的方法对语法树进行遍历，计算属性文法的所有属性值
* LL\(1\)：自上而下分析方法，深度优先建立语法树

**L- 属性文法**

* 对于每个产生式 $$A \rightarrow X_1X_2 \cdots X_n$$ ，其中每个语义规则中的每个属性要么是 **综合属性**，要么是 $$X_j(1 \le j \le n)$$ 的一个继承属性且这个继承属性仅依赖于：
  * 产生式中 $$X_j$$ 左边符号 $$X_1, X_2, \cdots , X_{j-1}$$ 的属性
  * A 的继承属性
* **S- 属性文法** 一定是 **L- 属性文法**

### 4.1  翻译模式

**语义规则**：给出了属性计算的定义，没有属性计算的次序等实现细节

**翻译模式**：给出了使用语义规则进行计算的次序，这样就可把某些实现细节表示出来

在翻译模式中，和文法符号相关的属性和语义规则（语义动作），用花括号 { } 括起来，插入到产生式右部的合适位置上

### 4.2 建立翻译模式

* 当只需要**综合属性**时：为每一个语义规则建立一个包含赋值的动作，并**把这个动作放在相应的产生式右边的末尾**
  * 产生式： $$T \rightarrow T_1 * F$$ 
  * 语义规则： $$T.val := T_1.val * F.val$$ 
  * 建立产生式和语义动作： $$T \rightarrow T_1 * F  \{T.val := T_1.val * F.val\}$$ 
* 如果既有**综合属性**又有**继承属性**，在建立翻译者模式时就必须保证：
  1. 产生式右边的符号的**继承属性**必须在这个符号以前的动作中计算出来
  2. 一个动作不能引用这个动作的右边的符号的**综合属性**
  3. 产生式左边非终结符的**综合属性**只有在它所引用的所有属性都计算出来以后才能计算。计算这种属性的动作通常可放在产生式右端的**末尾**

     **错误**：$$S \rightarrow A_1A_2 \{ A_1.in := 1; A_2.in := 2 \}  \\ A \rightarrow a \{ print(A.in) \} $$ ****

**例子：**

| **产生式** | 语义规则 | 翻译后的结果 |
| :--- | :--- | :--- |
| $$S \rightarrow B$$  | $$B.ps := 10 \\ S.ht := B.ht$$  | $$\begin{aligned} S \rightarrow \{&B.ps := 10\}  \\ &B \{ S.ht := B.ht \} \end{aligned}$$  |
| $$B \rightarrow B_1B_2$$  | $$B_1.ps := B.ps \\ B_2.ps := B.ps \\ B.ht := max(B_1.ht, B_2.ht)$$  | $$\begin{aligned}  S \rightarrow \{&B_1.ps := B.ps\}  \\  &B_1 \{ B_2.ps := B.ps \} \\ &B_2 \{ B.ht := max(B_1.ht, B_2.ht) \}  \end{aligned}$$  |
| $$B \rightarrow B_1  sub  B_2$$  |  $$B_1.ps := B.ps \\ B_2.ps := shrink(B.ps) \\ B.ht := disp(B_1.ht, B_2.ht)$$  | $$\begin{aligned}  S \rightarrow \{&B_1.ps := B.ps\}  \\  &B_1 sub  \{ B_2.ps := shrink(B.ps) \} \\ &B_2 \{ B.ht :=disp(B_1.ht, B_2.ht) \}  \end{aligned}$$  |
| $$B \rightarrow text$$  | $$B.ht := text.h * B.ps$$  | $$B \rightarrow text \{ B.ht := text.h * B.ps \}$$  |

* 把所有的语义动作都放在产生式的末尾
  * 语义动作的执行时机统一
* 转换方法
  * 加入新的产生式 $$M \rightarrow \epsilon$$ 
  * 把嵌入在产生式中的每个语义动作用不同的标记非终结符 M 代替，并把这个动作放在产生式 $$M \rightarrow \epsilon$$ 的末尾

$$\begin{aligned} E \rightarrow &TR \\ R \rightarrow &+ T \{ print('+') \} R \\ \mid &- T \{ print('-') \} R \\ \mid &\epsilon \\ T \rightarrow & num \{ print(num.val) \} \end{aligned}$$          $$\underrightarrow{\text{转换后}}$$       $$\begin{aligned}  E \rightarrow &TR \\  R \rightarrow &+ T M R \mid - TNR \mid \epsilon \\  T \rightarrow & num \{ print(num.val) \} \\ M \rightarrow & \epsilon \{ print('+') \} \\ N \rightarrow & \epsilon \{ print('-') \} \\ \end{aligned}$$ 

### 4.3 自顶向下翻译

* 动作是在处于相同位置上的符号被展开（匹配成功）时执行的
* 为了构造不带回溯的自顶向下语法分析，必须消除文法中的左递归
* 当消除一个翻译模式的基本文法的左递归时同时考虑**属性** —— 适合带**综合属性**的翻译模式

\*\*\*\*

**一般方法**

假设有翻译模式： $$A \rightarrow A_1 Y \{A.a := g(A_1.a, Y.y)\} \\ A \rightarrow X \{A.a := f(X.x)\}$$ 

它的每个文法符号都有一个综合属性，用小写字母表示，g 和 f 是任意函数

* 消除左递归： $$A \rightarrow X R  \\ R \rightarrow YR \mid \epsilon $$ 
* 翻译模式变为： $$\begin{aligned} A \rightarrow &X \{R.i := f(X.x)\} \\ &R \{ A.a := R.s \} \\ R \rightarrow &Y \{R_1.i := g(R.i, Y.y)\} \\ &R_1 \{ R.s := R_1.s \} \\ R \rightarrow &\epsilon \{ R.s := R.i \}  \end{aligned}$$ 

  其中 R.i ：R 前面子表达式的值；R.s ：分析完 R 时子表达式的值

![](.gitbook/assets/image%20%2881%29.png)



**构造抽象语法树的属性文法定义转化成翻译模式**

| 产生式 | 语义规则 |
| :--- | :--- |
| $$E \rightarrow E_1 + T$$  | $$E.nptr := mknode('+', E_1.nptr, T.nptr)$$  |
| $$E \rightarrow E_1 - T$$  | $$E.nptr := mknode('-', E_1.nptr, T.nptr)$$  |
| $$E \rightarrow T$$  | $$E.nptr := T.nptr$$  |
| $$T \rightarrow (E)$$  | $$T.nptr := E.nptr$$  |
| $$T \rightarrow id$$  | $$T.nptr := mkleaf(id, id.entry)$$  |
| $$T \rightarrow num$$  | $$T.nptr := mkleaf(num, num.val)$$  |

先对上述产生式进行消除左递归后：   

| 产生式 | 翻译模式 |
| :--- | :--- |
| $$E \rightarrow TR$$  | $$\begin{aligned} E \rightarrow & T \{ R.i := T.nptr \} \\ & R \{ E.nptr := R.s \}  \end{aligned}$$  |
| $$R \rightarrow +TR_1$$  | $$\begin{aligned} R \rightarrow & -T \{ R_1.i := mknode('+', R.i, T.nptr) \} \\ & R_1 \{ R.s := R_1.s \}  \end{aligned}$$  |
| $$R \rightarrow -TR_1$$  | $$\begin{aligned} R \rightarrow & -T \{ R_1.i := mknode('-', R.i, T.nptr) \} \\ & R_1 \{ R.s := R_1.s \}  \end{aligned}$$  |
| $$R \rightarrow \epsilon$$  | $$R \rightarrow \{ R.s := R.i \}$$  |
| $$T \rightarrow (E)$$  | $$T \rightarrow (E) \{ T.nptr := E.nptr \}$$  |
| $$T \rightarrow id$$  | $$T \rightarrow id \{ T.nptr := mkleaf(id, id.entry) \}$$  |
| $$T \rightarrow num$$  | $$T \rightarrow num \{ T.nptr := mkleaf(num, num.val) \}$$  |

![](.gitbook/assets/image%20%2883%29.png)

### 4.4 递归下降翻译器的设计



