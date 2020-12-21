# Problem233 数字1的个数

## 题目描述

给定一个整数n,计算所有小于等于n的非负整数中数字1出现的个数。

## 思路

暴力法肯定过不了复杂度的要求。

使用数学法，按照个位数、十位数......的顺序计算。我自己思路搞明白了，但是没有写出代码来，主要是没有很好的用代码表示当前位置的数字、高位数字以及低位数字。

看了题解的一个答案，参考着写了出来。

```python
class Solution:
    def countDigitOne(self, n: int) -> int:
        #思路 分别计算各个位置上1出现的次数 最后求和
        if n <= 0:
            return 0
        res = 0
        #base用来表示当前是计算哪个位置，比如说base等于1就表示是计算个位数，base=10表示当前计算十位数
        base = 1
        while n // base != 0:
            #cur_num表示当前位置上的数字
            cur_num = (n // base) % 10
            #high_num表示高位的数字，比如说当前计算十位数出现1的个数，那么1812中high_pos就应该是18
            high_num = n // (base * 10)
            #low_num表示低位的数字，比如说上面的例子就是2
            low_num = n - (n // base) * base
            #接下来就是比较当前数字和1的大小
            #1.如果=0，那么当前位置出现1的个数只取决于高位
            #2.如果=1，那么当前位置出现1的个数不光取决于高位，还取决于低位
            #3.如果>1,那么当前位置出现1的个数只取决于高位
            if cur_num == 0:
                res += high_num * base
            elif cur_num == 1:
                res += (high_num * base + (low_num + 1))
            else:
                res += (high_num + 1) * base
            #计算下一个位置的1的个数，base需要*10
            base *= 10
        return res
```

时间复杂度为log(n)，也就是数字的长度。空间复杂度是常数级别的。