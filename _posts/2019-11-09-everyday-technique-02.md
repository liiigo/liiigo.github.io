---
title: 每日技术笔记02 | 爬虫相关/朴素贝叶斯文本分类的思路/python路径相关
tags: 每日技术笔记
key: everydayTechnique02
comment: true
lang: zh
author: liigo
show_edit_on_github: false
pageview: true
show_subscribe: false
---
内容概览：爬虫，朴素贝叶斯，文本分类，python路径，markdown中latex公式预览

<!--more-->

因为自己总是在一些大家似乎没有问题的点上纠结，又不好意思问别人（…），就还是记下来自己多想一想。感觉每天的笔记其实都不需要有标题…

# 爬虫相关
## 根据特定标签抓取信息
观察网页[War and Peace](http://www.pythonscraping.com/pages/warandpeace.html)，可以发现一个标签结构：
```html
# 类名为class_name的行内元素
<span class="class_name"><span>

e.g.
# 红色字是说的话
<span class="red">Well, Prince, so Genoa and Lucca are now just family estates of the
Buonapartes. But I warn you, if you don't tell me that this means war,
if you still try to defend the infamies and horrors perpetrated by
that Antichrist- I really believe he is Antichrist- I will have
nothing more to do with you and you are no longer my friend, no longer
my 'faithful slave,' as you call yourself! But how do you do? I see
I have frightened you- sit down and tell me all the news.</span>

# 绿色字是人名
<span class="green">St. Petersburg</span>
```
于是可以用网页中的class来提取信息（以按出场顺序得到人名为例）：
```python
from urllib.request import urlopen
from bs4 import BeautifulSoup
html = urlopen("http://www.pythonscraping.com/pages/warandpeace.html")
bsObj = BeautifulSoup(html.read())
# 替代: bsObj = BeautifulSoup(html)
nameList = bsObj.findAll("span", {"class": "green"})
for name in nameList:
    print(name)
    # 结果: <span class="green">Baron Funke</span>
    print(name.get_text())
    # 结果: Baron Funke
```
**tagObj.get_text()**：清除所有HTML标签，返回只包含文字的字符串（建议尽可能保留标签结构，除非最后要用到单独的文字时）

一些获得属性的方法（得到的是Tag对象）：
- 直接调用子标签：**bsObj.tag**
- 返回找到的第一个标签：**bsObj.find(tag, attributes)**
- 获取所有指定标签：**bsObj.findAll(tag, attributes)**

注意find和findAll还有很多参数，之后用到了再具体学习。

## HTML标签结构的处理
拿到一个网页后，先搞清楚它的标签结构。
- 子标签（父标签下一级)：**.children**
- 后代标签（父标签下面所有级别）：**.descendants**
- 兄弟标签（不包含标签本身）：**.next_sibling**，**.next_siblings**，**.previous_sibling**，**.previous_siblings**，处理表格信息好用
- 父标签：**.parent**，**.parents**

# 用朴素贝叶斯进行文本分类
一个比较完整的教程，用来实现作业的第一部分：

[NLP系列(2)_用朴素贝叶斯进行文本分类(上)](https://blog.csdn.net/han_xiaoyang/article/details/50616559)

[NLP系列(3)_用朴素贝叶斯进行文本分类(下)](https://blog.csdn.net/longxinchen_ml/article/details/50629110)

朴素贝叶斯的**朴素**在于**条件独立性**，就是说：
$$ f(x_1, x_2, ..., x_n | G = j) = \prod_{i=1}^nf_{i}(x_i|G=j) $$


```
注：相乘符号的latex公式是\prod
```

贝叶斯定理出现的频率实在是太高了，但好像一直没有什么特别深入的理解，改天专门写一篇笔记讲讲这个吧。

# python相关
## 改变当前目录
```python
import os
os.chdir("/Users/<username>/Desktop")
```
顺便说明一下路径相关的问题：
- 当前目录："file"
- 父目录："../file"
- 根目录："/file"和"\file"都可接受
  
```python
# 当前文件夹的绝对路径
os.path.abspath('.')
# 当前文件夹上一级的绝对路径
os.path.abspath('..')
```

# 其他
这里单独说一下，vscode的markdown preview不支持latex语法的预览，网上查到的解决方案：
1. 装**markdown** **math**插件，亲测没有用处……
2. 装**markdown** **preview** **enhancement**，这个可以了，但是右侧预览不适配我的夜间主题，也是有点麻烦，先凑合用着吧。

二者都使用了[KaTeX](https://katex.org/)（解释是一个专门用于web的快速数学公式渲染工具库，我大概不会直接使用到它），官网给的例子看上去比MathJax要快很多。

但是jekyll似乎不是直接支持latex的，[How to support latex in github-pages?](https://stackoverflow.com/questions/26275645/how-to-support-latex-in-github-pages)提供了一些解决方案，之后研究一下。

后续：修改head.html文件的办法是好用的！