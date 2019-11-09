---
title: 每日技术笔记01 | 爬虫的异常处理/博客上传的步骤
tags: 每日技术笔记
key: everydayTechnique01
comment: true
lang: zh
author: liigo
show_edit_on_github: false
pageview: true
show_subscribe: false
---
内容概览：爬虫，异常处理，git，技术文档写作规范

<!--more-->

今天讲子图同构问题又翻到了csdn上一个id是“君的名字”的小姐姐的博客，博文总共有18页，一直在持续更新，多少有点受到激励。正好最近觉得自己的代码水平太差，写得太少，就不如每天写一些做个笔记，要等大块时间空出来再把东西一块一块学完大概是不可能了。现在通宵自习室只剩了五个人，安安静静写点代码还是挺好的。希望自己能坚持久一点。

# 爬虫的异常处理
## 打开网页时
打开一个网页：
```python
from urllib.request import urlopen
html = urlopen("http://www.pythonscraping.com/pages/page1.html")
```
打开一个网页会出现两种错误：
- 页面的错误：网页在服务器上不存在/获取页面出现错误，urlopen抛出**HTTPError**异常
- 服务器的错误：服务器不存在（链接打不开/写错了），urlopen返回**None**对象

检查这两种异常的代码：
```python
from urllib.error import HTTPError
try:
    html = urlopen("http://www.pythonscraping.com/pages/page1.html")
except HTTPError as e:
    # 显示错误内容
    print(e)
if html is None:
    print("URL is not found")
else:
    pass 
```
注意HTTPError是在urllib.error里的。

## 调用对象时
BeautifulSoup对象可以通过目标信息附近的标签直接提取目标信息。

得到一个美丽汤对象：
```python
from bs4 import BeautifulSoup
# html.read()：获取html的内容
bsObj = BeautifulSoup(html.read())
```
调用时可能出现的错误：
- 当调用一个不存在的标签时，美丽汤返回**None**对象
- 再调用这个None对象的子标签就会发生**AttributeError**

检查这两种异常的代码：
```python
try:
    badCount = bsObj.nonExistingTag.anotherTag
except AttributeError as e:
    print("Tag was not found")
else:
    # 如果没有捕捉到异常则会执行else之后的语句
    if badCount == None:
        print("Tag was not found")
    else:
        print(badCount)
```
其中的try-except-else结构中的**else**自己没怎么用过，以后可以试试。

在教材里一般会省略异常处理的部分，但是实际使用中还要多加注意。已经写好的带有完善异常处理的函数可以尝试复用。将上面两部分写成较完整的结构如下（以获得网页标题为例）：
```python
from urllib.request import urlopen
from urllib.error import HTTPError
from bs4 import BeautifulSoup

def getTitle(url):
    try:
        html = urlopen(url)
    except HTTPError as e:
        return None
    try:
        bsObj = BeautifulSoup(html.read())
        title = bsObj.body.h1
    except ArithmeticError as e:
        # 如果服务器有问题，则html为None对象，html.read()抛出这个error
        # 如果body不存在，title句抛出这个error
        return None
    return title

title = getTitle("http://www.pythonscraping.com/pages/page1.html")
if title == None:
    print("Title could not be found")
else:
    print(title)
```
结果：
```
<h1>An Interesting Title</h1>
```

# 博客上传的步骤
同步新写好的博文只需要三条非常简单的命令：
```
git add .
git commit -m "commit-text"
git push origin master
```
修改文章也是用同样的流程上传，非常简单。
基本框架搞好之后基本不需要实时预览，所以可以不用安装jekyll，直接在win10上写好博文然后push到远端就可以了。

# 其他
看到一篇文章：[技术文档写作规范](https://www.jianshu.com/p/3b638180e42c)，最近很喜欢看这种规范类的文章，包括正在读的《编写可读代码的艺术》，感觉应该让需要写代码的人都大概浏览一下。以前自己也对代码规范毫不在意，现在才知道可读性强的代码最大的受益人其实是过了一段时间后的自己。    