---
title: 每日技术笔记03 | 机器学习结果评价
tags: 每日技术笔记
key: everydayTechnique03
comment: true
lang: zh
author: liigo
show_edit_on_github: false
pageview: true
show_subscribe: false
---
内容概览：真正例、假正例...

<!--more-->

今天开始复习统计计算，但是与自己约定好了每天多多少少要做下一点笔记，所以还是写一写。

# ML的2x2结果表
没用markdown打过表格，放一个教程在这里：[https://www.jianshu.com/p/74b6ebcac2cb](https://www.jianshu.com/p/74b6ebcac2cb)

| 真实\预测 | 正(P) | 反(N) |
| :--: | :--: | :--: |
| **正** | 真正例(TP) | 假反例(FN) |
| **反** | 假正例(FP) | 真反例(TN) |

预测出的正例共有TP+FP，真实的正例共有TP+FN。

**查准率(precision)**：检索出的信息（正例）有多少是用户感兴趣的（即预测的正例中有多少是真的）

$$ P = \frac{TP}{TP+FP} $$

**查全率(recall)**：用户感兴趣的信息中有多少被检索出来了（真实的正例中有多少被检测出来了，即有多少是真正例）

$$ R = \frac{TP}{TP+FN} $$

**F1度量**：综合考虑查准率和查全率

$$ F1 = \frac{2 \times P \times R}{P+R} $$

```
latex中乘号是\times
```

**F$_\beta$度量**：F1度量的一般形式，$\beta$表示查全率对查准率的相对重要性，$\beta>1$查全率影响更大，$\beta<1$查准率影响更大

$$ F_{\beta} = \frac{(1+\beta^2)\times P \times R}{(\beta^2\times P)+R} $$