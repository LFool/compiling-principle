# 自上而下分析法

## 1. 自上而下分析法思路

从文法开始符号出发，向下**推导，**推出句子

自上而下分析的主旨

* 针对输入串，试图用一切可能的办法，从文法开始符号出发，自上而下的为输入串建立一颗语法树
* 为输入串寻找一个最左推导

## 2. 自上而下分析法面临的问题

### 2.1 回溯

当一个非终结符用某一个候选匹配成功时，这种匹配可能是暂时的

如果匹配到后面，发现出错，则必须**回溯**到前面某一个状态

![](../.gitbook/assets/image%20%2860%29.png)

### 2.2 左递归问题

如果存在非终结符 $$P$$ ，满足 $$P \stackrel{+} \Longrightarrow P\alpha$$ ，如果刚好匹配到了 $$P$$ 处，则会陷入无限循环中

![](../.gitbook/assets/image%20%2856%29.png)

如果是右递归则不会出现该问题，因为扫描指针会一直在移动，而不是停止不动，只要扫描指针在移动，那么输入串一定会扫描完

## 3. LL\(1\) 分析法

**两个目标：克服回溯；消除左递归**

\*\*\*\*

### **3.1 消除左递归**

**一个文法可以消除左递归的条件**

* 不含以 $$\epsilon $$ 为右部的产生式
* 不含回路 $$P \stackrel{+} \Longrightarrow P$$ 

#### **3.1.2 直接消除左递归**

引例：

                             ****$$P \rightarrow P \alpha \mid \beta$$             ****$$\longrightarrow$$                   ****$$\begin{aligned}  P & \Rightarrow P \alpha \\       & \Rightarrow P \alpha \alpha \\        & \Rightarrow \cdots \cdots\\       & \Rightarrow P \alpha \cdots \alpha  \\  & \Rightarrow \beta \alpha \cdots \alpha  \\    \end{aligned}$$ 

消除左递归后： ****$$\begin{aligned} P &\rightarrow \beta P' \\ P' &\rightarrow \alpha P' \mid \epsilon \\ \end{aligned}$$          $$\longrightarrow$$                    ****$$\begin{aligned}   P & \Rightarrow  \beta P' \\       & \Rightarrow  \beta \alpha  P' \\     & \Rightarrow  \beta \alpha \alpha  P' \\        & \Rightarrow \cdots \cdots\\        & \Rightarrow \beta \alpha \cdots \alpha P'  \\   & \Rightarrow \beta \alpha \cdots \alpha  \\    \end{aligned}$$ 



**一般而言，假定** $$P$$ **，满足** $$P \rightarrow P \alpha_1 \mid P \alpha_2 \mid \dots \mid P \alpha_m \mid \beta_1 \mid \beta_2 \mid \dots \mid \beta_n$$ **，其中，每个** $$\alpha$$ **都不等于** $$\epsilon$$ **，每个** $$\beta$$ **都不以** $$P$$ **开头**

**消除左递归的通式：** $$\begin{aligned} P & \rightarrow \beta_1 P' \mid \beta_2 P' \mid \dots \mid \beta_n P' \\ P' & \rightarrow \alpha_1 P' \mid \alpha_2 P' \mid \dots \mid \alpha_m P' \mid \epsilon \end{aligned}$$ 



例：文法 $$G(E)$$ ：

$$\begin{aligned} E &\rightarrow E + T \mid T \\ T & \rightarrow T * F \mid F \\ F & \rightarrow (E) \mid i \end{aligned}$$                $$\underrightarrow{\text{消除左递归}}$$           $$\begin{aligned}  E &\rightarrow T E' \\ E' &\rightarrow +TE' \mid \epsilon \\ T & \rightarrow F T' \\  T' & \rightarrow *FT' \mid \epsilon \\ F & \rightarrow (E) \mid i \\ \end{aligned}$$ 

#### **3.1.2 间接消除左递归**

对于文法 $$G(S)$$ ：

$$\begin{aligned}  S &\rightarrow Q c \mid c \\  Q & \rightarrow R b \mid b \\  R & \rightarrow S a \mid a \\ \end{aligned}$$ 

虽然没有直接左递归，但 S、Q、R都是左递归的 $$S \Rightarrow Qc \Rightarrow Rbc \Rightarrow Sabc$$ 

![](../.gitbook/assets/image%20%2859%29.png)                 ![](../.gitbook/assets/image%20%2854%29.png)                 ![](../.gitbook/assets/image%20%2858%29.png) 

        ****$$\begin{aligned}  S &\rightarrow Q c \mid c \\  Q & \rightarrow R b \mid b \\  R & \rightarrow S a \mid a \\ \end{aligned}$$                         ****$$\begin{aligned}   S &\rightarrow Q c \mid c \\   Q & \rightarrow Sab \mid a b \mid b \\   R & \rightarrow S a \mid a \\ \end{aligned}$$                $$\begin{aligned}   S &\rightarrow Sabc \mid abc \mid bc \mid \ c \\   Q & \rightarrow Sab \mid a b \mid b \\   R & \rightarrow S a \mid a \\ \end{aligned}$$ 

### 3.2 消除左递归算法

1. 把文法 $$G$$ 的所有非终结符按任一种顺序排列成 $$P_1, P_2, \dots,P_n ;$$ 按此顺序执行
2.   > FOR i := 1 TO n DO
   >
   > BEGIN
   >
   >         FOR j := 1 TO i - 1 DO
   >
   >         把形如 $$P_i \rightarrow P_j \gamma$$ 的规则改写成
   >
   >         $$P_i \rightarrow \delta_1 \gamma \mid \delta_2  \gamma \mid \dots \mid \delta_k \gamma;$$ 
   >
   >         \(其中 $$P_j \rightarrow \delta_1  \mid \delta_2  \mid \dots \mid \delta_k$$ 是关于 $$P_j$$ 的所有规则\)
   >
   >         消除关于 $$P_i$$ 规则的直接左递归性
   >
   > END

3. 化简由 2 所得的文法，去除那些从开始符号出发永远都无法到达的非终结符产生的规则

### **3.3 举例**

\*\*\*\*$$\begin{aligned}  & P_1 &&\rightarrow P_2 a_1  \\    &P_2 &&\rightarrow P_3 a_2  \\    &P_i &&\rightarrow \cdots  \\    &P_{n - 1} &&\rightarrow P_n a_{n-1} \\    &P_n &&\rightarrow P_1 a_n \mid \gamma_n \\    \end{aligned}$$ ****

算法过程：

* > when i : 1 to n -1, no change
  >
  > when i = n:
  >
  >         j = 1; $$P_n \rightarrow P_1 a_n \mid \gamma_n  \Longrightarrow P_n \rightarrow P_2 a_1a_n \mid \gamma_n$$ 
  >
  >         j = 2; $$P_n \rightarrow P_2 a_1a_n \mid \gamma_n  \Longrightarrow P_n \rightarrow P_3  a_2a_1a_n \mid \gamma_n$$ 
  >
  >         $$\cdots\cdots$$ 
  >
  >         j = n - 1; $$P_n \rightarrow P_{n-1} a_{n-2}a_{n-3} \cdots a_1a_n \mid \gamma_n  \Longrightarrow P_n \rightarrow P_na_{n-1} a_{n-2} \cdots a_1a_n \mid \gamma_n$$

消除左递归后的结果：

$$P_n \rightarrow \gamma_n P_n'  \\  P_n' \rightarrow a_{n-1} a_{n-2} \cdots a_1a_n P_n' \mid \epsilon $$ 

### 3.4 消除回溯

确保做出的选择唯一正确，这样就可以避免回溯

在讨论消除回溯时，文法已经不存在左递归了



两个步骤：构建终结首符集 $$FIRST(\alpha)$$ 、构建 $$FOLLOW(A)$$ 



#### 构造 $$FIRST(\alpha)$$ 

$$FIRST(\alpha)$$ 的作用是：明确告诉下一个要匹配的字符哪一个非终结符可以推导出来，即保存每个非终结符可以推出的第一个字符的集合。

**构造前的处理：**提取公共左因子

假设 $$A$$ 的规则是： $$A \rightarrow \delta\beta_1 \mid \delta\beta_2 \mid \dots \mid \delta\beta_n \mid \gamma_1 \mid \gamma_2 \mid \dots \gamma_m$$ ，其中，每个 $$\gamma$$ 不以 $$\delta$$ 开头，那么就可以改写这些规则来消除功能左因子 $$\delta$$ ，改写后为： $$A \rightarrow \delta A' \mid \gamma_1 \mid \gamma_2 \mid \dots \gamma_m \\ A' \rightarrow \beta_1 \mid \beta_2 \mid \dots \mid \beta_n$$ 

**正式构造：**唯一原则，把能推出的第一个字符加入集合，如果能推出的第一个是非终结符，则先放着

构造规则：

* $$A \rightarrow * \Longrightarrow FIRST(A) = \{*\}$$ 
* $$A \rightarrow *T \Longrightarrow FIRST(A) = \{*\}$$ 
* $$A \rightarrow TE \Longrightarrow FIRST(A) = FIRST(T)$$ ，非 $$\epsilon $$ 元素
* $$A \rightarrow TEF$$ 
  * 若 $$FIRST(T)$$ 不含 $$\epsilon$$ ，则只把 $$FIRST(T)$$ 中的元素加入 $$FIRST(A)$$ 
  * 若 $$FIRST(T)$$ 含有 $$\epsilon$$ ，则先把 $$FIRST(T)$$ 中非 $$\epsilon$$ 元素加入 $$FIRST(A)$$ ，再把 $$FIRST(E)$$ 中非 $$\epsilon$$ 元素加入 $$FIRST(A)$$ 
  * 若 $$FIRST(E)$$ 含有 $$\epsilon$$ ，则把 $$FIRST(F)$$ 中非 $$\epsilon$$ 元素加入 $$FIRST(A)$$
  * 以此类推
  * 若 $$FIRST(T), FIRST(E), FIRST(F)$$ ，均含 $$\epsilon$$ ，则把 $$\epsilon$$ 加入 $$FIRST(A)$$ 
* 直到所有集合均不发生变化为止



#### 构造 $$FOLLOW(A)$$ 

紧跟非终结符 $$A$$ 后面的元素的集合

构造规则：

* 将 $$\#$$ 置于开始符号的 $$FOLLOW$$ 集合中
* $$E \rightarrow TE'$$ ，将 $$FIRST(E')$$ 中的元素加入到 $$FOLLOW(T)$$ 中
* $$A \rightarrow \alpha B\beta$$ ，若 $$FIRST(\beta)$$ 含有 $$\epsilon$$ ，则把 $$FOLLOW(A)$$ 中的元素加入到 $$FOLLOW(B)$$ 中
* $$A \rightarrow \alpha B$$ ，则把 $$FOLLOW(A)$$ 中的元素加入到 $$FOLLOW(B)$$ 中
* $$FOLLOW$$ 集合中永远不加 $$\epsilon$$ 

### 3.5 举例

![](../.gitbook/assets/image%20%2861%29.png)













