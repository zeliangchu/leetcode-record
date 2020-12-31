# Problem406 根据身高重建队列

## 题目描述

![image-20201231175505412](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201231175505412.png)

## 思路

![image-20201231175539778](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201231175539778.png)

这个解释的思路基本和我最初写的差不多，但是我写的很麻烦。

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda x:(-x[0], x[1]))
        print(people)
        n = len(people)
        dic = dict()
        for p in people:
            if p[0] not in dic:
                dic[p[0]] = [p[1]]
            else:
                dic[p[0]].append(p[1])
        print(dic)
        res = [[0,0] for _ in range(n)]
        left = [i for i in range(n)]
        print(left)
        heights = list(dic.keys())
        heights.sort(reverse = True)
        print(heights)
        while heights:
            height = heights.pop()
            print(height)
            pos = dic[height]
            print(pos)
            if len(pos) == 1:
                res[left[pos[0]]] = [height, pos[0]]
                left.pop(pos[0])
            else:
                for p in pos:
                    res[left[p]] = [height, p]
                for p in sorted(pos, reverse = True):
                    left.pop(p)
        return res
```

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        n = len(people)
        people.sort(key=lambda x:(x[0],-x[1]))
        Index=[i for i in range(n)]
        Resort=[[0,0] for _ in range(n)]
        for shortest in people:
            Resort[Index[shortest[1]]] = shortest
            Index.pop(shortest[1])
        return Resort
```

**复杂度分析：**

- 时间复杂度：O(nlogn) 排序用的时间
- 空间复杂度：O(n)