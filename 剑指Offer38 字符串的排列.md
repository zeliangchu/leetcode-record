# 剑指Offer38 字符串的排列

## 题目描述

输入一个字符串，打印出该字符串中字符的所有排列，你可以以任意顺序返回这个字符串，但是里面不能有重复元素。

## 思路

直接看代码吧，主要是理解一下那两个交换。

```python
class Solution:
    def permutation(self, s: str) -> List[str]:
        c = list(s)
        n = len(c)
        res = []
        def dfs(index):
            if index == n - 1:
                res.append(''.join(c))
                return 
            dic = set()
            for i in range(index, n):
                #剪枝 当字符串中存在重复元素的时候，排列方案中也存在重复元素，为了排除重复方案，需要在固定某位字符的时候，保证每种字符只在此位置固定一次，也就是遇到重复字符时不交换，直接跳过
                if c[i] in dic:
                    continue
                dic.add(c[i])
                #这个题最应该注意的是这个交换，其实相当于一种回溯，先交换，交换的目的是将不同的字符固定到当前的index上，就相当于我们找全排列的过程中的a b c/a c b 固定a一样，后面再交换回来
                c[index], c[i] = c[i], c[index]
                dfs(index + 1)
                c[index], c[i] = c[i], c[index]
        dfs(0)
        return res
```

**复杂度分析：**

![image-20210103160923649](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210103160923649.png)