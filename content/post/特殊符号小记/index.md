---
title: 特殊符号小记
description: 用于编写md等文件时的特殊符号查阅
slug: special_chars
date: 2024-05-26 11:30:00+0800
image: imgs/char.jpg
categories:
    - techStudy
tags:
    - char
    - markdown
# math: 
# license: 
hidden: false
comments: true

draft: false

#links:
#  - title: 
#    description: 
#    website: 
#    image: 

#password: op

#passwordPoint: 这篇推文太_ _了
---

# 前言

由起是用md做高数笔记时候，遇到各种数学符号，每次都上网查阅太麻烦了，索性集中在这里吧。

# 符号

## 数学符号

### 希腊字母

| 小写 | 大写 | 名称 | latex(小写，大写首字母大写即可，如`\beta`->`\Beta)`|
| ---- | ---- | ---- | ---- |
| α | A | alpha | \alpha |
| β | B | beta | \beta |
| γ | Γ | gamma | \gamma |
| δ | Δ | delta | \delta |
| ε | E | epsilon | \epsilon |
| ζ | Z | zeta | \zeta |
| η | H | eta | \eta |
| θ | Θ | theta | \theta |
| ι | I | iota | \iota |
| κ | K | kappa | \kappa |
| λ | Λ | lambda | \lambda |
| μ | M | mu | \mu |
| ν | N | nu | \nu |
| ξ | Ξ | xi | \xi |
| ο | O | omicron | o |
| π | Π | pi | \pi |
| ρ | P | rho | \rho |
| σ | Σ | sigma | \sigma |
| τ | T | tau | \tau |
| υ | Y | upsilon | \upsilon |
| φ | Φ | phi | \phi |
| χ | X | chi | \chi |
| ψ | Ψ | psi | \psi |
| ω | Ω | omega | \omega |

### 运算符号

| 符号 | 名称 | latex |
| ---- | ---- | ---- |
| ∃ | 存在 | \exists |
| ∀ | 任意 | \forall |
| ∈ | 属于 | \in |
| ∅ | 空集 | \emptyset |
| ∞ | 无穷 | \infty |
| ∩ | 交集 | \cap |
| ∪ | 并集 | \cup |
| ⊂ | 包含于 | \subset |
| ⊃ | 包含 | \supset |
| ⊆ | 子集 | \subseteq |
| ⊇ | 包含 | \supseteq |
| ∵ | 因为 | \because |
| ∴ | 所以 | \therefore |
| ⇔ | 充要条件 | \Leftrightarrow | 

# 数学表达式的latex写法

## 简单表达式

| 表达式 | 名称 | latex |
| ---- | ---- | ---- |
| $lim_{x→a}f(x)$ | 极限 | lim_{x→a}f(x) |
| $a_b$ | 下标 | a_b |
| $a^b$ | 上标 | a^b |
| $\frac{a}{b}$ | 分数 | \frac{a}{b} |
| $\sqrt{a}$ | 开方 | \sqrt{a} |
| $\sqrt[n]{a}$ | n次方根 | \sqrt[n]{a} |
| $\sum_{i=1}^{n}a_i$ | 求和 | \sum_{i=1}^{n}a_i |
| $\prod_{i=1}^{n}a_i$ | 求积 | \prod_{i=1}^{n}a_i |
| $\int_{a}^{b}f(x)dx$ | 积分 | \int_{a}^{b}f(x)dx |
| $\iint_{D}f(x,y)dxdy$ | 二重积分 | \iint_{D}f(x,y)dxdy |
| $\iiint_{D}f(x,y,z)dxdydz$ | 三重积分 | \iiint_{D}f(x,y,z)dxdydz |
| $\oint_{C}f(x,y)ds$ | 曲线积分 | \oint_{C}f(x,y)ds |
| $\lim_{x→a}f(x)$ | 极限 | \lim_{x→a}f(x) |
|

## 复杂表达式

1. 定义域分类讨论的函数表达式

- 表达式

$$
f(x) = 
\begin{cases} 
    x^2 & \text{if } x < 0 \\
    2x + 1 & \text{if } 0 \leq x \leq 1 \\
    \frac{1}{x} & \text{if } x > 1
\end{cases}
$$

- latex

```latex
$$
f(x) = 
\begin{cases} 
    x^2 & \text{if } x < 0 \\
    2x + 1 & \text{if } 0 \leq x \leq 1 \\
    \frac{1}{x} & \text{if } x > 1
\end{cases}
$$
```








