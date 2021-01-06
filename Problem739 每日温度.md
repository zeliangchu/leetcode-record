# Problem739 每日温度（单调栈）

## 题目描述

![image-20210105230824806](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210105230824806.png)

## 思路1 BF

```python
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        n = len(T)
        for i in range(n):
            j = i + 1
            while j < n:
                if T[j] > T[i]:
                    T[i] = j - i
                    break
                else:
                    j += 1
                    continue
            if j == n:
                T[i] = 0
        return T
```

超时

## 思路2 另一种形式的BF

这种BF其实就是把上面的第二个循环变成了遍历温度

```python
from collections import defaultdict
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        n = len(T)
        ans = [0] * n
        nxt = dict()
        big = 10 ** 9
        for i in range(n -1,  -1, -1):
            warmer_index = min(nxt.get(t, big) for t in range(T[i]+1, 102))#注意这里是102 因为前面有可能是101
            if warmer_index != big:
                ans[i] = warmer_index - i
            nxt[T[i]] = i
        return ans
```

![image-20210106112017601](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210106112017601.png)

## 思路3 单调栈

以下解释基于官方题解。

可以维护一个存储下标的单调栈，从栈顶到栈底的下标对应的温度列表中的温度依次递减。如果一个下标在单调栈中说明尚未找到下次温度更高的下标。

正向遍历温度列表，对于温度列表中的每个元素T[i],如果栈为空，直接将i进栈，如果栈不为空，则比较栈顶下标对应的温度和当前温度，如果当前温度高，就可以将栈顶的下标弹出，并且将该下标对应的答案赋值为当前温度对应的下标和栈顶下标之间的差值，重复上述操作直到栈为空或者栈顶下标对应的温度小于等于当前温度，然后将进栈。

由于单调栈满足从栈底到栈顶的元素对应的温度依次递减，因此每次有元素进栈的时候，会将温度更低的元素全部移除，并更新出栈元素的等待天数，这样可以保证等待天数一定是最小的。

最好理解的方式就是通过一个实例来看看单调栈变化的过程，从而更好地理解这种方法。

```python
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        length = len(T)
        ans = [0] * length
        stack = []
        for i in range(length):
            temperature = T[i]
            while stack and temperature > T[stack[-1]]:
                prev_index = stack.pop()
                ans[prev_index] = i - prev_index
            stack.append(i)
        return ans
```

![image-20210106112033494](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210106112033494.png)

## 链接 Problem42-接雨水 Problem84-柱状图中最大的矩阵