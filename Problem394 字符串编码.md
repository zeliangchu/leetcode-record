# Problem394 字符串编码

## 题目描述

给定一个经过编码的字符串，返回它解码后的字符串。

![image-20201231130400741](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201231130400741.png)

## 思路1 栈

```python
class Solution:
    def decodeString(self, s: str) -> str:
        #使用栈来解决这个题
        #s为空返回
        if not s:
            return s
        #s全为字母返回
        if s.isalpha():
            return s
        stack = []
        for char in s:
            #只要不是]就入栈
            if char.isdigit() or char.isalpha() or char == '[':
                stack.append(char)
            #是]
            else:
                #先弹出字母
                tmp = ''
                while stack[-1] != '[':
                    tmp = stack.pop() + tmp
                #现在把括号之间所有的字符都弹出,然后弹出左括号
                stack.pop()
                #接下来弹出数字
                number = ''
                while stack and stack[-1].isdigit():
                    number = stack.pop() + number
                number = int(number)
                sub_string = number * tmp
                print(sub_string)
                stack.append(sub_string)
        return ('').join(stack)
```

**复杂度分析：**

- 时间复杂度：O(S) S表示解码后得出的字符串的长度
- 空间复杂度：O(S)

其实这个题也可以使用两个栈，一个数字栈，一个字符串栈，遇到有括号的时候弹出一个数字栈，字母栈弹出到左括号为止。

使用栈还有另一种写法。

```python
class Solution:
    def decodeString(self, s: str) -> str:
        stack, res, multi = [], "", 0
        for c in s:
            if c == '[':
                stack.append([multi, res])
                res, multi = "", 0
            elif c == ']':
                cur_multi, last_res = stack.pop()
                res = last_res + cur_multi * res
            elif '0' <= c <= '9':
                multi = multi * 10 + int(c)            
            else:
                res += c
        return res
```

这种写法的思路是下面这样。

![image-20201231133516098](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201231133516098.png)

## 思路2 递归

总体思路与辅助栈法一致，不同点在于将 [ 和 ] 分别作为递归的开启与终止条件：

- 当 s[i] == ']' 时，返回当前括号内记录的 res 字符串与 ] 的索引 i （更新上层递归指针位置）；
- 当 s[i] == '[' 时，开启新一层递归，记录此 [...] 内字符串 tmp 和递归后的最新索引 i，并执行 res + multi * tmp 拼接字符串。
  遍历完毕后返回 res。

```python
class Solution:
    def decodeString(self, s: str) -> str:
        def dfs(s, i):
            res, multi = "", 0
            while i < len(s):
                if '0' <= s[i] <= '9':
                    multi = multi * 10 + int(s[i])
                elif s[i] == '[':
                    i, tmp = dfs(s, i + 1)
                    res += multi * tmp
                    multi = 0
                elif s[i] == ']':
                    return i, res
                else:
                    res += s[i]
                i += 1
            return res
        return dfs(s,0)
```

