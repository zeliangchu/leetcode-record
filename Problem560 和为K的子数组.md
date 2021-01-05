# Problem560 和为K的子数组

## 题目描述

![image-20210105163253811](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210105163253811.png)

## 思路1 BF

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        n = len(nums)
        if n == 0:
            return 0
        res = 0
        for i in range(n):
            tmp = k
            if i != n - 1:
                for j in range(i, n):
                    tmp -= nums[j]
                    if tmp == 0:
                        res += 1
            else:
                if nums[i] == k:
                    res += 1
        return res
```

时间复杂度：O(n^2)

空间复杂度：O(1)

超时

## 思路2 前缀和＋哈希表

先看这一篇题解理解一下。

https://leetcode-cn.com/problems/subarray-sum-equals-k/solution/dai-ni-da-tong-qian-zhui-he-cong-zui-ben-fang-fa-y/

然后看这个题解和代码。

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        # num_times 存储某“前缀和”出现的次数，这里用collections.defaultdict来定义它
        # 如果某前缀不在此字典中，那么它对应的次数为0
        num_times = collections.defaultdict(int)
        num_times[0] = 1  # 先给定一个初始值，代表前缀和为0的出现了一次
        cur_sum = 0  # 记录到当前位置的前缀和
        res = 0
        for i in range(len(nums)):
            cur_sum += nums[i]  # 计算当前前缀和
            if cur_sum - k in num_times:  # 如果前缀和减去目标值k所得到的值在字典中出现，即当前位置前缀和减去之前某一位的前缀和等于目标值
                res += num_times[cur_sum - k]
            # 下面一句实际上对应两种情况，一种是某cur_sum之前出现过（直接在原来出现的次数上+1即可），
            # 另一种是某cur_sum没出现过（理论上应该设为1，但是因为此处用defaultdict存储，如果cur_sum这个key不存在将返回默认的int，也就是0）
            # 返回0加上1和直接将其置为1是一样的效果。所以这里统一用一句话包含上述两种情况
            num_times[cur_sum] += 1
        return res
```

前缀和刚开始做并不好理解，自己到时候多理解一下。

复杂度:

O(n)和O(n)