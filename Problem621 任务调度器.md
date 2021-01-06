# Problem621 任务调度器

## 题目描述

<img src="C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210106175936826.png" alt="image-20210106175936826" style="zoom:100%;" />

## 思路 利用桶的思想来解释这个题

直接看这篇题解，写的很棒。利用桶的思想来解决这个问题。

https://leetcode-cn.com/problems/task-scheduler/solution/tong-zi-by-popopop/

其实这个问题的关键就是明白一点：**完成所有任务的最短时间取决于出现次数最多的任务数量。**

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        counter = collections.Counter(tasks)
        maxinum = 0 #记录最多任务的数量
        count = 0 #记录有多少个是最多任务
        for v in counter.values():
            if v > maxinum:
                maxinum = v
                count = 1
            elif v == maxinum:
                count += 1
        return max(len(tasks), (n+1)*(maxinum-1)+count)
```

