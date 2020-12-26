# Problem367 有效的完全平方数

## 题目描述

给定一个正整数num，编写一个函数，如果num是一个完全平方数，则返回True，否则返回False。

## 思路 使用恒等式

恒等式的证明：

假设n>=2,则
$$
（n+1)^2-n^2=(n+1-n)(n+n+1)=2n+1
$$

$$
即A_{n+1} = A_n+2n+1=A{n-1}+(2n-1)+(2n+1)=1+3+5+...+2n+1= (n+1)^2
$$

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        #使用恒等式
        i = 1
        while num > 0:
            num -= i
            i += 2
        return num == 0
```

**复杂度分析:**

- 时间复杂度：
  $$
  O(n^{1/2})
  $$

从前面的递推共识可以看出对于一个完全平方数n，n等于前sqrt(n)个奇数相加的和，也就是while里面进行了sqrt(n)次运算。

- 空间复杂度：O(1)

