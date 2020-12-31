# Problem399 除法求值

## 题目描述

![image-20201231150004346](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201231150004346.png)

## 思路

这个题我最开始的想法就是类似于解方程的方法，但是自己没有关注下面的提示，如何用图来做？

先构造图，使用嵌套的字典实现，其天然的hash可以在in判断的时候做到O(1)的复杂度。

对于每个equation比如a/b=v构造a到b的带权v的有向边，和b到a的带权1/v的有向边。

之后对于每个queries，只需要进行dfs并将路径上的边权重叠乘就是结果了，如果路径不可达结果就是-1.

```python
class Solution:
    def calcEquation(self, equations, values, queries):
        #构造图 equations中的第一项除以第二项等于value中的对应值，第二项除以第一项等于其倒数
        graph = {}
        for (x, y), v in zip(equations, values):
            if x in graph:
                graph[x][y] = v
            else:
                #字典的Key是x,字典的键值是字典，保存的是y和对应的value
                graph[x] = {y:v}
            if y in graph:
                graph[y][x] = 1 / v
            else:
                graph[y] = {x: 1 / v}
        
        #dfs找寻从s到t的路径并返回结果叠乘后的边权重即结果
        def dfs(s, t):
            if s not in graph:
                return -1
            if t == s:
                return 1
            for node in graph[s].keys():
                if node == t:
                    return graph[s][node]
                #访问过了就接着遍历
                if node in visited:
                    continue
                #没访问过，就进行深度优先搜索 看看能不能找到一条路径
                if node not in visited:
                    #添加到访问过，防止重复访问
                    visited.add(node)
                    #从当前节点开始进行深度优先遍历
                    v = dfs(node, t)
                    #如果路径通了，返回结果
                    if v != -1:
                        return graph[s][node]*v
                    #否则路径没通，就接着遍历graph中的节点
                    else:
                        continue
                #如果遍历了一圈没有找到结果，返回-1
            return -1
            
        res = []
        for qs, qt in queries:
            visited = set()
            res.append(dfs(qs, qt))
        return res
```

