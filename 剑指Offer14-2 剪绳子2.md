# 剑指Offer14-2 剪绳子2

## 题目描述

这个题和上一个题目的区别在于这个题使用了需要取模，就是最后的结果需要取模1e^9+7，其实关于为什么很多这种题都需要对1e^9+7进行取模也是一个很有意思的事情，这个数满足一些条件，自己可以去网上查。

## 思路

这个题经过前一个题的铺垫之后思路已经比较清晰了，最简单的方法就是直接最后取模。

注意，这种方法在Python中是可行的，但是在别的语言中可能存在越界的问题，Python内部自带大整数运算能力，整数运算不会溢出，只要内存足够就可以了（因为整数溢出的本质就是空间不够完整地存放数据，Python对此处理的方法就是加空间）。

```python
class Solution:
    def cuttingRope(self, n: int) -> int:
        if n < 4:
            return n - 1
        a, b = divmod(n , 3)
        if b == 0:
            return pow(3, a) % 1000000007
        if b == 1:
            return (pow(3, a - 1) * 4) % 1000000007
        return (pow(3, a) * 2) % 1000000007
```

其实这个pow函数可以直接进行n次方之后的取模操作，就是下面这样。

```python
class Solution:
    def cuttingRope(self, n: int) -> int:
        if n < 4:
            return n - 1
        a, b = divmod(n , 3)
        if b == 0:
            return pow(3, a, 1000000007)
        if b == 1:
            return (pow(3, a - 1, 1000000007) * 4) % 1000000007
        return (pow(3, a, 1000000007) * 2) % 1000000007
```

我们可以发现上面主要耗时的是求幂的过程，正常求一个数的多少方是O(N)的时间复杂度，其实N是指数的大小。

我们可以使用更高级的算法——快速幂算法。这个算法之前在本科的时候上应用密码学的时候是学过的。

<img src="C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210126152503697.png" alt="image-20210126152503697" style="zoom:50%;" />

在讲RSA加密算法的时候，因为需要进行指数很大的幂运算。

**简单地说，这个算法的核心思想就是每一步把指数分成两半，而相应的底数做平方运算。这样就把非常大的指数给不断地缩小，所需要执行的循环次数也不断的变小。**

实现快速幂的算法如下：

```python
#实现快速幂算法
    def quickPow(self, base, power):
        result = 1
        while power > 0:
            #如果指数为偶数
            if power % 2 == 0:
                power //= 2#把指数缩小为原来的一半
                base = (base * base) % 1000000007
            #如果指数为奇数
            else:
                power = power - 1
                result = result * base % 1000000007#因为为奇数，所以需要乘以一次base 
                power //= 2
                base = (base * base) % 1000000007
        return result
```

还能写的再简单些。

```python
#实现快速幂算法
    def quickPow(self, base, power):
        result = 1
        while power > 0:
            if power % 2 == 1:
                result = result * base % 1000000007
            power //= 2
            base = (base * base) % 1000000007
        return result
```

再进行优化，就是使用位运算优化power的有关运算。比如说，判断奇数偶数我们就可以使用&1来进行判断，因为1的二进制码再计算机中是0b00000001，只有最后一个比特是1，对数据进行与操作，如果最后得到的结果是1,说明这个数是奇数，如果最后的结果是0，说明这个数字是偶数。

同时除2的操作还可以通过位运算来实现。power>>1

```python
 #实现快速幂算法
    def quickPow(self, base, power):
        result = 1
        while power > 0:
            if power & 1 == 1:
                result = result * base % 1000000007
            power >>= 1
            base = (base * base) % 1000000007
        return result
```

最后整个题的代码如下所示。

```python
class Solution:
    def cuttingRope(self, n: int) -> int:
        if n < 4:
            return n - 1
        a, b = divmod(n , 3)
        if b == 0:
            return self.quickPow(3, a) % 1000000007
        if b == 1:
            return (self.quickPow(3, a - 1) * 4) % 1000000007
        return (self.quickPow(3, a) * 2) % 1000000007

    #实现快速幂算法
    def quickPow(self, base, power):
        result = 1
        while power > 0:
            if power & 1 == 1:
                result = result * base % 1000000007
            power >>= 1
            base = (base * base) % 1000000007
        return result
```

其实也可以在定义快速幂函数的是由引入模数。无所谓了。这样就将幂运算的时间复杂度优化到了O(logN)，其实N=a。

知道了快速幂算法之后，顺便Problem372https://leetcode-cn.com/problems/super-pow/也就解决了。