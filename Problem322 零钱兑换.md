# Problem322 零钱兑换

**这道题算一道经典题，评论区很多人说在腾讯二面出现过。**

## 题目描述

给定不同面额的硬币coins和一个总金额。编写一个函数来计算可以凑成总金额所需的最少的硬币的个数。如果没有任何一种硬币组合能组成总金额，返回-1.

## 思路一 使用回溯算法+剪枝

其实这个题和组合问题的第一题很像，只不过组合问题需要给出具体的组合，这个只需要找出长度最小的组合即可。

我按照组合问题的套路写了一个回溯算法，超时了，而且卡在了很靠前的用例上。

然后我考虑用贪心算法，将coins数组从大到小进行排序，就是一直优先选最大的硬币，当不能再选最大的硬币的时候再找后面的，但是这种方法得到的结果不一定是最后要求解的答案。举例来说：考虑到有 `[1,7,10]` 这种用例，按照贪心思路 `10 + 1 + 1 + 1 + 1` 会比 `7 + 7` 更早找到，所以很明显还是要把所有的情况都递归完。

题解中有一个讲这个方法的写的还不错。

https://leetcode-cn.com/problems/coin-change/solution/322-by-ikaruga/

这个解法中的回溯函数使用了几个参数：

- coins数组
- amount当前剩下的需要凑出的数量
- c_index来标记当前遍历到哪个coin了
- count来记录使用的硬币数目
- ans作为全局变量来记录最后的结果

她的解法的一个关键是**除法对加法的一个加速**：

- 我们虽然是优先对面值比较大的硬币进行尝试，但是其实我们没有必要一个个地进行尝试，可以用除法算一下最多能使用几个大硬币。
  - k=amount / coins[c_index] 计算最大能使用几个当前硬币
  - amount - k * coins[c_index] 减去扔了几个硬币
  - count+k 加k个硬币

C++代码如下所示：

```C++
void coinChange(vector<int>& coins, int amount, int c_index, int count, int& ans)
{
    if (amount == 0)
    {
        ans = min(ans, count);
        return;
    }
    if (c_index == coins.size()) return;
	//k + count < ans剪枝 因为如果加上k之后的count 比当前的ans大，就没有接着计算的必要了
    for (int k = amount / coins[c_index]; k >= 0 && k + count < ans; k--)
    {
        coinChange(coins, amount - k * coins[c_index], c_index + 1, count + k, ans);
    }
}

int coinChange(vector<int>& coins, int amount)
{
    if (amount == 0) return 0;
    sort(coins.rbegin(), coins.rend());
    int ans = INT_MAX;
    coinChange(coins, amount, 0, 0, ans);
    return ans == INT_MAX ? -1 : ans;
}
```

python代码如下所示

```python
class Solution(object):
    def coinChange(self, coins, amount):
        def coinChanging(coins, amount, c_index, count, ans):
            if amount == 0:
                return min(ans, count)
            if c_index == len(coins):
                return ans
            k = amount // coins[c_index]
            while k >= 0 and k + count < ans:
                ans = coinChanging(coins, amount - k * coins[c_index], c_index + 1, count + k, ans)
                k -= 1
            return ans
        if amount == 0:
            return 0
        coins.sort(reverse=True)
        ans = coinChanging(coins, amount, 0, 0, float('inf'))
        return ans if ans != float('inf') else -1
```

## 思路2 把这个问题当作背包问题来看待

**背包问题其实算是动态规划下面的一个子问题，首先可以看一下关于完全背包问题的这篇文章。**

**https://labuladong.gitbook.io/algo/dong-tai-gui-hua-xi-lie/1.3-bei-bao-lei-xing-wen-ti/bei-bao-ling-qian**

**这篇文章其实是使用的零钱兑换2来做的例子。自己可以好好品味一下。**

**关于这个零钱兑换问题感觉自己还是有点乱。后面再好好梳理一下。**

**下面的题解是零钱兑换1中一个参考labuladong模板的写法，这个写法可以和上面零钱兑换2的写法对应起来，作为一套模板。待梳理！**

**https://leetcode-cn.com/problems/coin-change/solution/wan-quan-bei-bao-wen-ti-gan-xie-labuladongda-shen-/**

## 思路3 动态规划

下面写的解法是一个自下而上的思路来做的，其实也就是先通过计算小的面值的dp结果然后一步步转移出大的面值的结果。

我们使用实例1来分析：

coins = [1, 2, 5] amount = 11

凑成面值为11的最少硬币个数可以由以下三者的最小值得到：

- 面值为1的硬币+凑成面值为10的最少硬币的个数
- 面值为2的硬币+凑成面值为9的最少硬币的个数
- 面值为5的硬币+凑成面值为6的最少硬币的个数

也就是说dp[11] =min(dp[11-1]+1, dp[11-2]+1, dp[6]+1)

### 定义状态

dp[i]表示凑齐面值为i所需要的最小的硬币个数，len(dp)= amount + 1

### 状态转移方程

```python
dp[amount] = min(dp[amount], 1+dp[amount - coins[i]]) for i in [0, len-1] if coins[i] < amount
```

**注意：**

- if语句就是为了保证当前硬币的面值要小于要凑出来的面值
- 有可能面临的一个问题是dp[amount - coins[i]]这个值找不出来，这个时候我们应该把这个数字设置为amout+1,其实也就是在初始化的时候将dp所有的元素设置为amount+1(有例外 dp[0]=0)，这样就保证了在使用状态转移方程进行计算的时候如果碰到凑不出来的，就会使用他们的初始值，一个不可能的数字，而且因为使用的是min比较，赋值amount+1也是合理的。

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [amount + 1 for _ in range(amount + 1)]
        dp[0] = 0
        for i in range(1, amount + 1):
            for coin in coins:
                #if语句的第一个条件是为了保证当前硬币的面值要小于要凑出的面值
                #if语句的第二个条件是看dp[i-coin]如果=amount+1说明它凑不出来，就没有计算的必要了
                if coin <= i and dp[i - coin] != amount + 1:
                    dp[i] = min(dp[i], dp[i-coin] + 1)
        if dp[amount] == amount + 1:
            dp[amount] = -1
        return dp[amount]
```

**复杂度分析：**

- 时间复杂度：O(N*amount) N是可选硬币的种类数  amount是面值
- 空间复杂度：O(amount)，状态数组的大小为amount

