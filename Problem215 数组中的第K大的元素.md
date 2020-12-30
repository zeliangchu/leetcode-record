# Problem215 数组中的第K大的元素

## 题目描述

在未排序的数组中找到第k大的元素。

## 思路1 快速排序

然后这个题的第一种方法就是快速排序，首先回顾一下**快速排序**。

**快速排序算法步骤：**

1. **从数列中挑出一个元素，称为”基准“(pivot)**
2. **重新排列数列所有元素比基准值小的放在基准的前面，所有元素比基准值打的放在基准的后面（相同的数字可以到任意一边），这样这个基准就位于世俗列的中间位置。这个操作称为partition切分操作**
3. **递归地把小于基准值地子数列和大于基准值的子数列进行排序**

**说明：**

1. **基准值的选定可以是随机选择一个，一般选择第一个元素**
2. **切分操作总能排定一个元素是，并且还能知道这个元素最终所在的位置，这样经过一次切分操作之后就能缩小搜索的范围，这样的思想就叫做”减而治之”。这里的切分操作的算法才是快速排序的关键，这里切分操作的步骤是将pivot位置选择为left(最左端),然后选取index=pivot+1，这里的index是用来标记所有比pivot指向的元素小的元素的最右端的位置，最后需要将pivot指向的元素和index指向的元素进行交换，这样就能够保证pivot左边的元素都是相对于pivot指向的元素小的。然后，下标i从index开始遍历，当当前元素小于基准指向的元素的时候，将i指向的元素和index指向的元素进行交换，然后index加1。遍历完了之后将pivo指向的元素和index-1指向的元素进行交换。（注意这里一定是index-1，因为index+1之后的元素是比pivot指向的元素大的）**

接下来使用python实现快速排序。

```python
def quickSort(arr, left, right):
    left = 0 if not isinstance(left, (int, float)) else left
    right = len(arr) - 1 if not isinstance(right, (int, float)) else right
    if left < right:
        partitionIndex = partition(arr, left, right)
        quickSort(arr, left, partitionIndex - 1)
        quickSort(arr, partitionIndex + 1, right)
    return arr

def partition(arr, left, right):
    pivot = left
    index = pivot + 1
    i = index
    while i <= right:
        if arr[i] <= arr[pivot]:
            swap(arr, i, index)
            index += 1
        i += 1
    swap(arr, pivot, index - 1)
    return index - 1

def swap(arr, i, j):
    arr[i], arr[j] = arr[j], arr[i]
```

这个题我们可以使用快速排序来解决这个问题，这样平均时间复杂度就是O(nlogn)，但是这个题还有更好的方法。

我们回想一下快速排序的过程，每次经过划分之后，我们一定可以确定一个元素的最终位置，并且保证这个元素左边的元素小于这个元素，右边的大于这个元素，然后只要某次划分找到的这个元素的下标是倒数第k个下标的时候，就是最终的答案。代码如下。

时间复杂度:O(n) 证明过程可以参考「《算法导论》9.2：期望为线性的选择算法」

空间复杂度:O(1) 原地排序，没有耗费多余的空间

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        n = len(nums)
        target = n - k 
        left = 0
        right = n - 1
        while True:
            index1 = self.partition(nums, left, right)
            if index1 == target:
                return nums[index1]
            elif index1 < target:
                left = index1 + 1
            else:
                right = index1 - 1

    def partition(self, arr, left, right):
        pivot = left
        index = pivot + 1
        i = index
        while i <= right:
            if arr[i] <= arr[pivot]:
                self.swap(arr, i, index)
                index += 1
            i += 1
        self.swap(arr, pivot, index - 1)
        return index - 1

    def swap(self, arr, i, j):
        arr[i], arr[j] = arr[j], arr[i]
```

注意，我们还可以针对极端用例进行优化，因为我们知道快速排序的最坏情况就是要排序的数组是一个顺序数组，这样的递归树会像是一个链表一样，对这里的选择的操作也是一样的，我们可以通过随机化的方法打乱一下数组的顺序，这样就减少了极端测试用例的耗费时间。代码如下。提交之后可以发现时间快了不少。

```python
class Solution:
    import random
    def findKthLargest(self, nums: List[int], k: int) -> int:
        n = len(nums)
        target = n - k 
        left = 0
        right = n - 1
        while True:
            index1 = self.partition(nums, left, right)
            if index1 == target:
                return nums[index1]
            elif index1 < target:
                left = index1 + 1
            else:
                right = index1 - 1

    def partition(self, arr, left, right):
        random_index = random.randint(left, right)
        arr[random_index], arr[left] = arr[left], arr[random_index]

        pivot = left
        index = pivot + 1
        i = index
        while i <= right:
            if arr[i] <= arr[pivot]:
                self.swap(arr, i, index)
                index += 1
            i += 1
        self.swap(arr, pivot, index - 1)
        return index - 1

    def swap(self, arr, i, j):
        arr[i], arr[j] = arr[j], arr[i]
```

## **思路二：堆排序**

**堆的性质：**

- **堆在逻辑上是一颗完全二叉树**
- **堆是基于数组实现的，堆的所有元素都存储在数组中**
- **满足任意节点的值都大于其子树中的节点的值的堆，称为大堆**
- **满足任意节点的值都小于其子树中的节点的值的堆，称为小堆**
- **堆的基本作用就是快速地在集合中找到最值**

因而，这个题也可以用堆排序来解决，先建立一个大根堆，做k-1次删除操作后堆顶的元素就是最后要找的元素。在很多语言中都有优先队列或者堆（堆是优先队列的一种实现方式）的容器可以直接使用，但是在面试的时候我们最好还是自己实现一个堆，堆的建立、调整和删除的过程是非常重要的。

首先自己实现一个堆（最大堆）。堆的表示可以直接使用list，就是按照层序遍历的顺序将每个节点的值放在数组中，这里一个非常重要的点就是父节点和子节点之间的关系：

**parent = int((i-1) / 2)**

**left = 2 * i + 1**

**right = 2 * i + 2**

其中i表示数组的索引，如果left和right的值超出了数组的索引，那么表示这个节点是不存在的。

**堆的主要操作**：

1. ###### 往堆中插入值，sift-up操作

   ![image-20201206125400008](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201206125400008.png)
   2.获取或者删除根节点

   ![image-20201206125500043](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201206125500043.png)

堆的实现代码如下：

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        n = len(nums)
        Heap = MaxHeap(n)
        for i in nums:
            Heap.add(i)
        for _ in range(k):
            res = Heap.extract()
        return res

class MaxHeap(object):
    def __init__(self, maxsize = None):
        self.maxsize = maxsize
        self._elements = [None for _ in range(maxsize)]
        self._count = 0

    def __len__(self):
        return self._count

    def add(self, value):
        if self._count >= self.maxsize:
            raise Exception('full')
        self._elements[self._count] = value
        self._count += 1
        self._siftup(self._count - 1)

    def _siftup(self, ndx):
        if ndx > 0:
            parent = int((ndx - 1) / 2)
            if self._elements[ndx] > self._elements[parent]:#如果插入的值大于parent的值
                self._elements[ndx], self._elements[parent] = self._elements[parent], self._elements[ndx]
                #递归地进行处理
                self._siftup(parent)
    
    def extract(self):
        if self._count <= 0:
            raise Exception('empty')
        value = self._elements[0]#保存root的值
        self._count -= 1
        self._elements[0] = self._elements[self._count]#将最右下方的节点放到root之后siftdown，这里非常容易出错！！！
        self._siftdown(0)#调整根节点的位置
        return value
    
    def _siftdown(self, ndx):
        left = ndx * 2 + 1
        right = ndx * 2 + 2
        larger = ndx#用来标记子节点中的较大的节点

        if(left < self._count and self._elements[left] > self._elements[larger] and self._elements[left] > self._elements[right]):#首先一定要有左节点，这个判断语句一定不要忘了，然后需要左节点比根节点和右节点的值都大
            larger = left
        elif right < self._count and self._elements[right] > self._elements[larger]:
            larger = right
        else:
            larger = ndx
        if larger != ndx:
            self._elements[larger], self._elements[ndx] = self._elements[ndx], self._elements[larger]
            self._siftdown(larger)
```

简单写法：

时间复杂度：O(nlogn)，建堆的时间代价是 O(n)，删除的总代价是 O(klogn)，因为k<n，故渐进时间复杂为 O(n + k log n) = O(nlogn)。
空间复杂度：O(log n），即递归使用栈空间的空间代价。

另外给出一个其他的写法

```c++
class Solution {
public:
    void maxHeapify(vector<int>& a, int i, int heapSize) {
        int l = i * 2 + 1, r = i * 2 + 2, largest = i;
        if (l < heapSize && a[l] > a[largest]) {
            largest = l;
        } 
        if (r < heapSize && a[r] > a[largest]) {
            largest = r;
        }
        if (largest != i) {
            swap(a[i], a[largest]);
            maxHeapify(a, largest, heapSize);
        }
    }

    void buildMaxHeap(vector<int>& a, int heapSize) {
        for (int i = heapSize / 2; i >= 0; --i) {
            maxHeapify(a, i, heapSize);
        } 
    }

    int findKthLargest(vector<int>& nums, int k) {
        int heapSize = nums.size();
        buildMaxHeap(nums, heapSize);
        for (int i = nums.size() - 1; i >= nums.size() - k + 1; --i) {
            swap(nums[0], nums[i]);
            --heapSize;
            maxHeapify(nums, 0, heapSize);
        }
        return nums[0];
    }
};
```

## 关联 Problem347 前k个高频元素