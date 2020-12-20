# Problem231 2的幂

## 题目描述

给定一个整数，编写一个函数来判断它是否为2的幂次方。

## 思路一  使用bin函数转换为字符串之后计算字符串中"1"的个数

代码如下：

```python
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        if n <= 0:
            return False
        if bin(n).count('1') > 1:
            return False
        else:
            return True
```

## 思路二 使用位运算:去除二进制中最右边的1

```python
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        if n <= 0:
            return False
        if n & (n - 1) == 0:
            return True
        else:
            return False
```

## 思路三 使用位运算：获取二进制中最右边的1

```python
class Solution(object):
    def isPowerOfTwo(self, n):
        if n == 0:
            return False
        return n & (-n) == n
```

这里需要了解一下补码的知识。