# 剑指Offer21 调整数组顺序使奇数位于偶数前面

## 题目描述

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

## 思路1 不使用额外的空间 双指针

```python
class Solution:
    def exchange(self, nums: List[int]) -> List[int]:
        #使用额外空间很简单,如果不适用额外空间的话
        n = len(nums)
        #使用两个指针，只要左指针为偶数，右指针为奇数，就将两者交换
        if n == 1:
            return nums
        left = 0
        right = 1
        while right < n:
            #如果左右指针指向的数字都是奇数，左右指针同时向右移动
            if self.isodd(nums[left]) and self.isodd(nums[right]):
                left += 1
                right += 1
            #如果左右指针指向的数字都是偶数，那么右指针一直右移直到指向的数字是奇数，然后将右指针指向的数字和左指针指向的数字互换
            elif not self.isodd(nums[left]) and not self.isodd(nums[right]):
                while right < n and not self.isodd(nums[right]):
                    right += 1
                if right == n:
                    break
                nums[left], nums[right] = nums[right], nums[left]
                #这里需要注意一下，互换之后左指针指向的是奇数，右指针指向的是偶数，并且左指针（不包含）到右指针之间都是偶数，直接将左指针+1,然后进入新的情况左右指针都是偶数
                left += 1
            #如果左指针是偶数，右指针是奇数,直接互换，然后左右指针同时向右移动
            elif not self.isodd(nums[left]) and self.isodd(nums[right]):
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
                right += 1
            #如果左指针是奇数，右指针是偶数，继续
            else:
                left += 1
                right += 1
        return nums

    def isodd(self, num):
        if num % 2:
            return True
        return False
```

时间复杂度为O(n)，空间复杂度为O(1)

## 思路 和上面一样使用双指针

我居然没有想到左右指针从两端开始遍历这种最简单的思路...

```python
class Solution:
    def exchange(self, nums: List[int]) -> List[int]:
        left, right = 0, len(nums)-1
        while left <= right:
            if nums[left] % 2 == 1:
                left += 1
            elif nums[right] % 2 == 0:
                right -= 1
            else:
                temp = nums[left]
                nums[left] = nums[right]
                nums[right] = temp
                left += 1
                right -= 1
        return nums
```

还有一个居然忘了使用&1来判断奇偶性，不应该。

## 思路 还有一种快慢指针的说法

其实和我第一种思路有点类似，只不过这个思路更简洁。

![image-20210127225248058](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210127225248058.png)

```python
class Solution:
    def exchange(self, nums: List[int]) -> List[int]:
        n = len(nums)
        low, fast = 0, 0
        while fast < n:
            if nums[fast] & 1:
                nums[low], nums[fast] = nums[fast], nums[low]
                low += 1
            fast += 1
        return nums
```

