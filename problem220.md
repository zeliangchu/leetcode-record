# 2020.12.8号 力扣第220题 存在重复元素3

## 题目描述

在整数数组nums中，是否存在两个下标i和j，使得nums[i]和nums[j]的差的绝对值小于等于t，而且满足i和j的差的绝对值小于等于k。

如果存在则返回true，不存在返回false。

## 思路1 暴力解法

超时。

## 思路2 使用桶

官方题解的桶方法的思想。

受到桶排序的思想的启发，我们可以把桶当作一个窗口来实现一个线性复杂度的解法。桶排序的思想是把元素分散到不同的桶中，然后接着每个桶再独立地用不同的排序算法（比如插入排序）进行排序。（桶内部可以是一个链表，每次进行插入的操作）

**桶排序的步骤：**

1. 根据待排序集合中的最大元素和最小元素的差值范围和映射规则确定申请的桶的个数。
2. 遍历排序序列，将每个元素放到对应的桶里面去。
3. 对不是空的桶进行排序。
4. 按顺序访问桶，将桶中的元素依次放回到原来序列中对应的位置，完成排序。

![img](https://pic3.zhimg.com/v2-b29c1a8ee42595e7992b6d2eb1030f76_b.webp)

```python
def bucket_sort(nums, bucketsize):
	min_num = min(nums)
	max_num = max(nums)
	#桶的大小
	bucket_count = (max_num - min_num + 1) // bucketsize
	#桶数组
	bucket_lists = [[] for i in range(bucketcount)]
	
	for i in nums:
		ndx = (i - min_num) // bucketsize
		bucket_lists[ndx].append(i)
	
	nums.clear()
	for i in bucket_lists:
		for j in sorted(i):
			nums.append(j)
```

![image-20201209200613342](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201209200613342.png)

那么我们回到这个题上，这个题我们需要解决的最大的问题在于：

​	1.对于给定的元素x，在窗口中是否存在区间[x-t,x+t]内的元素？

​	2.我们都在常量时间内完成上面的判断吗？

官方题解中给了一个很好的例子就是生日的例子。假如你的生日在三月的某一天，你想知道班级上是否有人和你的生日的间隔在一个月之内，这里假设每个月都是30天，那么我们只要检查二月、三月和四月就可以了。这里其实就是把一个月当作一个桶，每个桶的区间范围都是t，很显然，任何不在同一个桶或者相邻的两个桶的两个元素之间的距离一定是大于t的。

基于以上思想，我们在本题中设计一些桶，让他们分别包含区间[0,t],[t+1,2t+1]，这样我们只需要检查x所属于的那个桶和相邻桶中的元素就可以了。

代码如下：

```python
class Solution:
    def containsNearbyAlmostDuplicate(self, nums: List[int], k: int, t: int) -> bool:
        if t < 0 or k < 0:
            return False
        all_buckets = {}
        bucket_size = t + 1#这里将桶的大小定义为t+1是因为这样才能满足桶内包含的数字的差值的绝对值<=t
        for i in range(len(nums)):
            ndx = nums[i] // bucket_size#计算桶的下标
            if ndx in all_buckets:#这个位置的桶已经存在了（已经被放进去元素了）
                return True
            all_buckets[ndx] = nums[i]#放入桶中
            if (ndx - 1) in all_buckets and abs(all_buckets[ndx-1] - nums[i]) <=t:#检查前一个桶
                return True
            if (ndx + 1) in all_buckets and abs(all_buckets[ndx+1] - nums[i]) <= t:#检查后一个桶
                return True
            if i >= k:
                all_buckets.pop(nums[i-k] // bucket_size)
         return False
```

**复杂度分析：

时间复杂度：O(n) 对于这n个元素中的任意一个来说，我们最多只需要在散列表中做三次搜索，一次插入和一次删除。这些操作是常量时间复杂度的。因此整个算法的时间复杂度是O(n)。

空间复杂度：O(min(n,k)) 需要开辟的额外空间取决于散列表的大小，散列表的大小的上限取决于n和k中的较小值。