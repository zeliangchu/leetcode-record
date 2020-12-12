# 2020.12.12号 力扣第223题 矩形面积

## 题目描述

在二维平面上计算出两个由直线构成的矩形重叠后形成的总面积。每个矩阵由其左下顶点和右上顶点坐标表示。

## 思路

```python
class Solution:
    def computeArea(self, A: int, B: int, C: int, D: int, E: int, F: int, G: int, H: int) -> int:
        #先求解重叠部分的左下角的点的坐标，取两个矩形的左下角的横纵坐标的最大值
        left_down_x = max(A,E)
        left_down_y = max(B,F)
        #再求解重叠部分的右上角的点的坐标，取两个矩阵的右上角的横纵坐标的最小值
        right_up_x = min(C,G)
        right_up_y = min(D,H)
        #如果出现重叠部分的左下角的横坐标大于等于右上角的横坐标，或者左下角的纵坐标大于等于右上角的纵坐标，说明没有重合
        if left_down_x >= right_up_x or left_down_y >= right_up_y:
            return (C - A) * (D - B) + (G -E) * (H - F)
        else:
            return (C - A) * (D - B) + (G -E) * (H - F) - (right_up_x - left_down_x) * (right_up_y - left_down_y)
```

复杂度分析：时间复杂度和空间复杂度都是常数级别。