---
layout: page
title: 概率论读书笔记
description: >
  南开大学《概率论》读书笔记，概念摘要和重点习题记录
hide_description: false
sitemap: false
---

快速定位：[概念摘要](#概念摘要)
, [习题](#习题)

1. 列表（占位作用）
{:toc}

## 概念摘要
### 非测度论体系定义（古典概率和几何概型）
- 随机试验：进行一次试验，如果其所得结果不能完全预言，但其全体可能结果是已知的，则称此试验为随机试验
- 样本点：随机试验的每个可能结果称为一个样本点
- 随机试验决定了样本点和样本空间
- 事件/随机事件：由部分样本点组成的试验结果为随机事件，简称作事件
- 当样本空间为又穷或至多可列无穷的集合时，可取其任何子集为事件。而当样本空间为不可列无穷时，只能取 $$\Omega$$ 的一部分
性质较好的（可测的）子集作为随机事件，详见[测度论体系定义](#测度论体系定义)

### 测度论体系定义
- 测度：在一个给定集合 $$\Omega$$ 的某一子集 $$\rm C$$ 上定义的一个满足完全可加性的非负函数 $$\mu(A)$$
- Lebesgue测度实例：常见的1维情形的长度、2维情形的面积、3维情形的体积等
- 事件/随机事件：
由样本空间 $$\Omega$$ 的某些子集组成的类 $$\mathscr{F}$$，如果满足：\\
  - 样本空间 $$\Omega \in \mathscr{F}$$
  - 若 $$A \in \mathscr{F}$$，则 $$\overline{A}\in\mathscr{F}$$  (对逆封闭) 
  - 若一切 $$A_n\in \mathscr{F}$$，则 $$\bigcup\limits^\infty_{n=1} \in \mathscr{F}$$  (对可列并封闭)\\
，则称 $$\mathscr{F}$$ 为 $$\Omega$$ 上的事件 $$\sigma$$ 代数，而 $$\mathscr{F}$$ 中的集合称作随机事件，简称事件
- 概率满足：
  - 非负性
  - 规范性：$$P(\Omega)=1$$
  - 可列可加性：对任何两两不相容的事件列$${A_n}$$
  ，有 $$P(\bigcup\limits^\infty_{n=1}A_n)=\displaystyle\sum^\infty_{n=1}P(A_n)$$
- Borel集：设 $$\Omega=R$$， 取全体半直线组成的类\\
$$\mathscr{P}=\{(-\infty,x): -\infty<x<+\infty \} $$\\
，则 $$\mathscr{B}=\sigma(\mathscr{P})$$ 称为Borel集类，而 $$\mathscr{B}$$ 中集合为Borel集。
集合的并、交及逆运算，称为Borel运算
- 当 $$\Omega$$ 是R的某个子区间时，我们可用 $$\Omega$$ 交上述Borel子集类 $$\mathscr{B}$$ 中每一个集，
并将得到的类（i.e., $$\Omega$$ 中的Borel集类）作为 $$\Omega$$ 上的事件 $$\sigma$$ 代数
- 一维几何概率空间：
样本空间 $$\Omega$$ 是 $$(-\infty, +\infty)$$ 中的Borel集，具有正的有限Lebesque测度。
事件 $$\sigma$$ 代数 $$\mathscr{F}$$ 取作 $$\Omega$$ 中的Borel集类。 对每个 $$A \in \mathscr{F}$$，取 $$P(A)$$ 如：\\
$$P(A)=\frac{\mu(A)}{\mu(\Omega)}$$\\
，于是得到几何概型的概率空间 $$(\Omega, \mathscr{F}, P)$$

## 习题
### 1.5.4 （全概率公式、Bayes公式和递推方法）
#### 习题5 
- 红球与黑球各5个混放在一起，将它们任意等分放入甲乙两袋中，再从两袋各任意取出一个球，求两球同色的概率

以两球同色事件为 $$A$$ ，同时基于对称性，以两袋中的红黑组合 -- 各5红5黑（$$B_1$$），各2红3黑或3红2黑（$$B_2$$），各4红1黑或1红4黑（$$B_3$$）分割样本空间，可得

$$
\begin{aligned} 
P(A) &=P(AB_1)+P(AB_2)+P(AB_3) \\
     &=P(A|B_1).P(B_1)+P(A|B_2).P(B_2)+P(A|B_3).P(B_3) \\
	 &=0\times(\frac{C_5^5}{C_{10}^5}\times2)
	 +(2\times\frac{2}{5}\times\frac{3}{5})\times(\frac{C^2_5 \times C^3_5}{C^5_{10}}\times2)
	 +(2\times\frac{4}{5}\times\frac{1}{5})\times(\frac{C^1_5 \times C^4_5}{C^5_{10}}\times2)\\
	 &=\frac{4}{9}
\end{aligned}
$$

#### 习题11
- 接连抛一枚均匀硬币，直至第一次出现两个相连的正面为止。求恰抛n次的概率

A为出现连续两个正面结束，所求为 $$P(An)$$，以第n次抛出正面（$$H_n$$）或背面（$$T_n$$）分割样本空间得

$$
\begin{aligned}
P(A_1) &=0 \\
P(A_2) &=\frac{1}{2}\times\frac{1}{2}=\frac{1}{4} \\
P(A_n) &=P(A_nH_1)+P(A_nT_1)\\
&=P(A_n|H_1)P(H_1)+P(A_n|T_1)P(T_1)\\
&=\frac{1}{2}(0\times P(A_{n-1}|H_2)+\frac{1}{2}\times P(A_{n-1}|T_2))+\frac{1}{2}\times P(A_{n-1}) \\
&=\frac{1}{4}P(A_{n-2})+\frac{1}{2}P(A_{n-1})
\end{aligned}
$$

设$$(P_n-xP_{n-1})=y(P_{n-1}-xP_{n-2})$$
，由根与系数关系解方程得（x,y）为（$$\frac{1+\sqrt{5}}{4}, \frac{1-\sqrt{5}}{4}$$）。
带入迭代公式，有

$$
\begin{aligned}
P(A_n)-xP(A_{n-1}) &=y^{n-2}(P(A_2)-xP(A_1)) \\
P(A_n) &=\frac{1}{4}y^{n-2}+xP(A_{n-1}) \\
&=\frac{1}{4}y^{n-2}x^0+\frac{1}{4}y^{n-3}x^1+...+\frac{1}{4}y^0x^{n-2}\\
&=\frac{1}{4}\cdot \frac{y^{n-2}(1-(\frac{x}{y})^{n-1})}{1-\frac{x}{y}}\\
&=\frac{1}{4}\cdot \frac{y^{n-1}-x^{n-1}}{y-x}\\
&=\frac{1}{2\sqrt{5}}[(\frac{1+\sqrt{5}}{4})^{n-1}-(\frac{1-\sqrt{5}}{4})^{n-1}]
\end{aligned}
$$


Back to [Some Notes](README.md){:.heading.flip-title}

