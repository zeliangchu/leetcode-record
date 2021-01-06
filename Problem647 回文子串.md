# Problem647 回文子串

## 题目描述

<img src="C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210106163827943.png" alt="image-20210106163827943" style="zoom:50%;" />

## 思路1 BF

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        n = len(s)
        res = n
        
        for i in range(n-1):
            for j in range(i+1, n):
                if self.check(s[i:j+1]):
                    res += 1
        return res
    
    def check(self, s):
        left, right = 0, len(s) - 1
        while left < right:
            if s[left] == s[right]:
                left += 1
                right -= 1
            else:
                return False
        return True
```

超时

## 思路2 从中间向两边扩散(中心扩展法)

其实这个思路应该要容易想到，因为回文串本质上就是一个关于中心对称的字符串，所以最直接的方法就应该是遍历中心，然后从中心向两边扩展。

遍历字符串，从当前字符（回文串长度为奇数）或者当前字符和下一个字符（回文串长度为偶数）作为中心向两边扩散来判断是不是回文串。

直接看代码。

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        n = len(s)
        self.res = 0
        for i in range(n):
            self.count(s, i, i)#回文串长度为奇数
            self.count(s, i, i+1)#回文串长度为偶数
        return self.res
    
    def count(self, s, start, end):
        while start >= 0 and end < len(s) and s[start] == s[end]:
            self.res += 1
            start -= 1
            end += 1
```

**复杂度分析：**

- 时间复杂度：O(n^2)
- 空间复杂度：O(1)

## 思路3 Manacher算法

待学习