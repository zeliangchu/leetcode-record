# 剑指Offer17-打印从1到最大的n位数

## 题目描述

输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

## 思路

这个题的关键考点是看大数问题。

https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/solution/mian-shi-ti-17-da-yin-cong-1-dao-zui-da-de-n-wei-2/

上面这个题解就写的不错，一步步深入，考虑大数的打印之后，使用递归来进行全排列，会面临需要面临两个问题：

- 删除高位多余的0
- 列表从1开始

```python
class Solution:
    def printNumbers(self, n: int) -> List[int]:
        def ds(x):
            if x == n:
                while self.start < n-1 and num[self.start] == '0':
                    self.start += 1
                if ''.join(num[self.start:]) != "0":
                    res.append(int(''.join(num[self.start:])))
                    self.start = 0
                return
            for i in range(10):
                num[x] = str(i)
                ds(x+1)
        num = [0] * n
        self.start = 0
        res = []
        ds(0)
        return res
```

