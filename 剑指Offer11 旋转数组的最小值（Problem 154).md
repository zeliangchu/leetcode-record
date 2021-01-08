# 剑指Offer11 旋转数组的最小值（Problem 154)

## 题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

## 思路1 一次遍历 时间复杂度O(n)

```python
class Solution:
    def minArray(self, numbers: List[int]) -> int:
        for i in range(1, len(numbers)):
            if numbers[i] < numbers[i-1]:
                return numbers[i]
            else:
                continue
        return numbers[0]
```

## 思路2 二分法 时间复杂度O(logn)

按照数组的中点将数组一分为二，总有一个数组是有序的，一个数组是无序的，最小元素就在这个无序数组中。

不断地二分从而找到最小元素？

```python
class Solution:
    def minArray(self, numbers: List[int]) -> int:
        n = len(numbers)
        left, right = 0, n - 1
        while left <= right:
            mid = left + (right - left) // 2
            #根据right和mid的关系来判断
            #如果right对应的元素比Mid对应的元素小 说明左边一定是有序的
            if numbers[right] < numbers[mid]:
                left = mid + 1
            #如果right对应的元素比mid对应的元素大，说明右边一定是有序的
            elif numbers[right] > numbers[mid]:
                right = mid#这里为什么一定要是right = mid  如果是mid - 1就不行呢 ？？？
            #如果right对应的元素和mid对应的元素相等，去重
            else:
                right -= 1
        return numbers[left]
```

解释一下为什么一定要是right = mid

![image-20210109013319026](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210109013319026.png)