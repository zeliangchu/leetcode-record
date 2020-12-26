# Problem283 移动零

## 题目描述

给定一个数组nums,编写一个函数将所有0移动到数组的末尾，同时保持非零元素的相对顺序。

## 思路1 从后往前将0弹出到一个数组中，最后合并数组

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if nums.count(0) == 0:
            return nums
        helper = []
        n = len(nums)
        for i in range(n - 1, -1, -1):
            if nums[i] == 0:
                helper.append(nums.pop(i))
        return nums.extend(helper)
```

**复杂度分析：**

- 时间复杂度:O(n)
- 空间复杂度:O(k) k代表0的个数

## 思路2 两次遍历（相当于双指针）

使用两个指针i和j，第一次i进行遍历的时候j指向的元素都用来记录非零元素，也就是说只要nums[i] != 0就把nums[i]赋值给nums[j]，然后j++,如果nums[i]==0，就不进行操作，i指针接着移动，等到第一次遍历结束的时候，j指针及其之后的位置就应该都是0了，然后改成0就可以了。

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if not nums:
            return 
        j = 0
        for i in range(len(nums)):
            if nums[i]:
                nums[j] = nums[i]
                j += 1
        for i in range(j, len(nums)):
            nums[i] = 0
```

**复杂度分析：**

- 时间复杂度:O(n)
- 空间复杂度:O(1)

## 思路3 一次遍历

这个思路主要参考的是快速排序的思想，快速排序需要确定一个待分割的元素作为中间点x，然后把所有小于x的元素放到x的左边，大于x的元素放到x的右边。

这里我们用的是0作为中间点，把不等于0的元素放到0的左边，等于0的放到0的右边。

下面的代码解释的比较好。

```java
 /**
     * 双指针法（参考快排）
     * @param nums
     */
    void moveZeroes1(int[] nums) {
//        初始化双指针
//        j指针用于找中间点0
        int j = 0;
//        i指针用于找中间点右侧非0元素
        for (int i = 0; i < nums.length; i++) {
//            如果两个指针指向元素均不为0，整体右移一位
            if (nums[j] != 0 && nums[i] != 0) {
                j++;
            }
//            如果j指针找到了中间点0，i没有找到中间点右侧非0元素
            else if(nums[j]==0&&nums[i]==0){
                continue;
            }
//            如果j指针找到了中间点0，i找到了中间点右侧非0元素
            else if(nums[j]==0&&nums[i]!=0){
                int temp=nums[j];
                nums[j]=nums[i];
                nums[i]=temp;
                j++;
            }


        }
    }
```

这是Python代码。

```python
class Solution(object):
	def moveZeroes(self, nums):
		"""
		:type nums: List[int]
		:rtype: None Do not return anything, modify nums in-place instead.
		"""
		if not nums:
			return 0
		# 两个指针i和j
		j = 0
		for i in xrange(len(nums)):
			# 当前元素!=0，就把其交换到左边，等于0的交换到右边
			if nums[i]:
				nums[j],nums[i] = nums[i],nums[j]
				j += 1
作者：wang_ni_ma
链接：https://leetcode-cn.com/problems/move-zeroes/solution/dong-hua-yan-shi-283yi-dong-ling-by-wang_ni_ma/
```

但是上面的代码一个问题是数组开头就是非零元素的话i==j，这个时候还是会进行交换，另一个就是交换的时候只需要把i的值赋给j并且将原来的位置的值置为0.所以不需要交换的操作，只需要赋值就可以了。

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if not nums:
            return 
        j = 0
        for i in range(len(nums)):
            if nums[i]:
                if i > j:
                    nums[j] = nums[i]
                    nums[i] = 0
                j += 1
```

