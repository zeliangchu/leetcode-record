# Problem279 完全平方数

## 题目描述

给定正整数n，找到若干个完全平方数，使得它们的和等于n，并且让组成和的完全平方数的个数最少。

以下思路主要基于下述题解。

这个题解有四种解法。

https://leetcode-cn.com/problems/perfect-squares/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by--51/

这个题解主要是Python实现了动态规划和BFS。

https://leetcode-cn.com/problems/perfect-squares/solution/dong-tai-gui-hua-bfs-zhu-xing-jie-shi-python3-by-2/

## 思路1 回溯法

相当于一种暴力的解法，去考虑所有的分解方案，找出最小的解，举个例子。

```
n = 12
先把n减去一个平方数，然后求剩下的数分解成平方数和所需要的最小个数

把n减去1，然后求出11分解成平方数和所需要的最小个数，记作n1
那么当前方案总共需要n1+1个平方数

把 n 减去 4, 然后求出 8 分解成平方数和所需的最小个数,记做 n2
那么当前方案总共需要 n2 + 1 个平方数

把 n 减去 9, 然后求出 3 分解成平方数和所需的最小个数,记做 n3
那么当前方案总共需要 n3 + 1 个平方数

下一个平方数是 16, 大于 12, 不能再分了。
```

代码的话，就是回溯的写法，或者说是 DFS。

```java
public int numSquares(int n) {
    return numSquaresHelper(n);
}

private int numSquaresHelper(int n) {
    if (n == 0) {
        return 0;
    }
    int count = Integer.MAX_VALUE;
    //依次减去一个平方数
    for (int i = 1; i * i <= n; i++) {
        //选最小的
        count = Math.min(count, numSquaresHelper(n - i * i) + 1);
    }
    return count;
}
```

python的代码待补充。

但是上面的代码会超时，因为很多解其实会重复地计算，我们需要用到memorization技术，也就是把过程中的解利用hashmap全部保存起来即可。

```java
public int numSquares(int n) {
    return numSquaresHelper(n, new HashMap<Integer, Integer>());
}

private int numSquaresHelper(int n, HashMap<Integer, Integer> map) {
    if (map.containsKey(n)) {
        return map.get(n);
    }
    if (n == 0) {
        return 0;
    }
    int count = Integer.MAX_VALUE;
    for (int i = 1; i * i <= n; i++) {
        count = Math.min(count, numSquaresHelper(n - i * i, map) + 1);
    }
    map.put(n, count);
    return count;
}
```

python代码待补充。

## 思路2 动态规划

![image-20201226002120666](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201226002120666.png)

```python
class Solution:
    def numSquares(self, n: int) -> int:
        dp=[i for i in range(n+1)]
        for i in range(2,n+1):
            for j in range(1,int(i**(0.5))+1):
                dp[i]=min(dp[i],dp[i-j*j]+1)
        return dp[-1]
```

## 思路3 BFS

有待学习

## 思路4 数学方法

这道题如果知道数学定理之后，相当于告诉你：

1. 任何正整数都可以拆分成不超过4个数的平方和 ---> 答案只可能是1,2,3,4
2. 如果一个数最少可以拆成4个数的平方和，则这个数还满足 n = (4^a)*(8b+7) ---> 因此可以先看这个数是否满足上述公式，如果不满足，答案就是1,2,3了
3. 如果这个数本来就是某个数的平方，那么答案就是1，否则答案就只剩2,3了
4. 如果答案是2，即n=a^2+b^2，那么我们可以枚举a，来验证，如果验证通过则答案是2
5. 只能是3