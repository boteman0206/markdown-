##### 1： list等分操作

```python
>>> from math import ceil
>>> def divide_iter(lst, n):
      if n <= 0:
        yield lst
        return
      i, div = 0, ceil(len(lst) / n)
      while i < n:
        yield lst[i * div: (i + 1) * div]
        i += 1

>>> for group in divide_iter([1,2,3,4,5],2):
      print(group)
# [1, 2, 3]
# [4, 5]
```

##### 2： yield实现斐波那契函数

![1594970883754](../picture\1594970883754.png)

##### 3:  if not x

```python
# 直接使用 x 和 not x 判断 x 是否为 None 或空
x = [1,3,5]
if x:
    print('x is not empty ')

if not x:
    print('x is empty')
```

