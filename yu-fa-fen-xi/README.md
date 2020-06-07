# 语法分析

> 语法分析的前提：
>
> * 采用正规式和有限自动机扫描和识别语言的单词符号
> * 使用上下文无关文法来描述语法规则

## 前情回顾

### 上下文无关文法

$$G = (V_T，V_N，P，S)$$ ****

* $$V_T$$ ：终结符集合（非空）
* $$V_N$$ ：非终结符集合（非空），$$V_T \bigcap V_N = \Phi$$ 
* $$P$$ ：产生式集合（有限）
  * 产生式的一般形式： $$\alpha \rightarrow\beta$$  读作： $$\alpha$$ 定义为 $$\beta$$
    * $$\alpha \in V_N$$
    * $$\beta \in (V_T \bigcup V_N) ^ *$$ 
* $$S$$ ：开始符号，$$S \in V_N$$ 
* 开始符 $$S$$ 至少必须在某个产生式的左部出现一次

### 句型

如果 $$S \stackrel{*}\Longrightarrow \alpha$$ ，则称 $$\alpha$$ 是一个句型

### 句子

仅含终结符的句型是一个句子

### 语言

文法 $$G$$ 所产生的句子的全体是一个语言，记为 $$L(G)$$ 

$$L(G) = \{ \alpha \mid S \stackrel{+}\Longrightarrow  \alpha, \alpha \in V_T^* \}$$ 

### Hint

$$\alpha _1 \stackrel{*}\Longrightarrow \alpha_n$$ ： $$\alpha _1$$ 经过 0 步或若干步到达 $$\alpha_n$$ 

$$\alpha _1 \stackrel{+}\Longrightarrow \alpha_n$$ ： $$\alpha _1$$ 经过 1 步或若干步到达 $$\alpha_n$$ 



