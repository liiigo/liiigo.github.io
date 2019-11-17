---
title: 每日技术笔记04 | assert, reload, 矩阵行标准化
tags: 每日技术笔记
key: everydayTechnique04
comment: true
lang: zh
author: liigo
show_edit_on_github: false
pageview: true
show_subscribe: false
---
内容概览：python assert, reload, np.linali.norm

<!--more-->

# python相关
## assert 断言
判断一个表达式，表达式为True正常执行，为False触发异常，终止程序
可以用于替代print调试法

```python
assert 1 == 2, '1不等于2'
```
结果：
```python
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError: 1 不等于 2
```

## 重新导入模块
写了一个模块，导入到shell里后再修改这个模块，重新import并不能使模块内容更新，因为python会认为“我已经导入了这个模块，所以不需要重新导入了”，所以采用reload():

```python
import word2vec
from importlib import reload

reload(word2vec)
```

- 在 Python 2.x 中，reload()是内置函数。
- 在 Python 3.0 - 3.3 中，可以使用imp.reload(module)
- 在 Python 3.4 中，imp已经被废弃，取而代之的是importlib

参考：[在python中重新导入模块](https://www.cnblogs.com/mlgjb/p/10282029.html)

## numpy：行标准化
数据标准化在机器学习和深度学习中很常见，因为数据标准化后梯度下降可以更快收敛。
标准化过程：将矩阵每行除以行向量的范数

```python
def normalize_rows(x: numpy.ndarray):
    """
    function that normalizes each row of the matrix x to have unit length.

    Args:
     ``x``: A numpy matrix of shape (n, m)

    Returns:
     ``x``: The normalized (by row) numpy matrix.
    """
    return x / numpy.linalg.norm(x, ord=2, axis=1, keepdims=True)
```

**numpy.linali**：numpy的线性代数函数库
**numpy.linali.norm**：
- ord默认为2（2范数）
- axis=1：按行操作
- keepdims：是否保持矩阵的二维特性（不保持：返回一个数；保持：返回一个nd.array）

参考：[Normalizing With Numpy](https://www.cnblogs.com/mlgjb/p/10282029.html)