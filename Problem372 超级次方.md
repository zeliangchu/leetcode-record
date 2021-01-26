# Problem372 超级次方

## 题目描述

你的任务是计算 a^b 对 `1337` 取模，`a` 是一个正整数，`b` 是一个非常大的正整数且会以数组形式给出。

## 思路 快速幂

```python
class Solution:
    def superPow(self, a: int, b: List[int]) -> int:
        power = 0
        base = 1
        for i in range(len(b) - 1, -1, -1):
            power += b[i] * base
            base *= 10
        result = 1
        while power > 0:
            if power & 1 == 1:
                result = result * a % 1337
            power >>= 1
            a = (a * a) % 1337
        return result
```

