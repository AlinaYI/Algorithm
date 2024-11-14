# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

Given an array of positive integers `nums` and a positive integer `target`, return *the **minimal length** of a* 

*subarray* *whose sum is greater than or equal to* `target`. If there is no such subarray, return `0` instead.

**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

**Example 2:**

```
Input: target = 4, nums = [1,4,4]
Output: 1
```

**Example 3:**

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

 

**Constraints:**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        '''
            这道题要求的是windows里面的值是等于target
            然后求最小的这样的windows
            这里要求是subarray，也就是需要是在nums里面相邻的windows

            tc: O(n) sc: O(1)
        '''

        # 首先像处理下edge case

        # 1. 如果整个nums的和都小于target
        #    那么说明不可能有这样的subarray存在
        #    对应的是example3
        if not nums or sum(nums) < target:
            return 0
        # 2. 如果整个nums的和加起来才等于target
        #    那么也就不用去找subarray了
        #    直接返回整个nums的长度就行
        elif sum(nums) == target:
            return len(nums)

        # 对于剩下其他情况的处理
        # subarray 的话可以用window来写
        left = right = 0
        res = float('inf')
        curr_sum = 0
        for right in range(len(nums)):
            # curr_sum 用来记录当前window里面的和
            curr_sum += nums[right]
            # 如果当前window里面的和大于或者等于target
            # 说明找到了想要的window
            # update res，要比较出最小的，然后记录
            # 因为往后面加，就只会大于target，所以这里调整left pointer
            while curr_sum >= target:
                res = min(res, right-left+1)
                curr_sum -= nums[left]
                left += 1
        return res



##############################################################
        '''
            要想要去实现一下Onlogn的tc 就可以用到prefixed+二分

        '''
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        n = len(nums)

        # 构建前缀和数组
        prefix_sums = [0] * (n + 1)
        for i in range(1, n + 1):
            prefix_sums[i] = prefix_sums[i - 1] + nums[i - 1]
        # 初始化结果为无穷大
        res = float('inf')

        # 遍历前缀和数组，使用二分查找
        for i in range(n):
            # 目标是找到最小的 j，使得 prefix_sums[j] - prefix_sums[i] >= target
            target_sum = prefix_sums[i] + target
            # 使用二分查找找到满足条件的最小 j
            j = bisect.bisect_left(prefix_sums, target_sum)

            # 如果找到的 j 是合法的，则更新最小长度
            if j <= n:
                res = min(res, j - i)
        # 如果没有找到满足条件的子数组，返回 0
        return 0 if res == float('inf') else res

```





# [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)

Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from `1` to `n2` in spiral order.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

```
Input: n = 3
Output: [[1,2,3],[8,9,4],[7,6,5]]
```

**Example 2:**

```
Input: n = 1
Output: [[1]]
```

 

**Constraints:**

- `1 <= n <= 20`



```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:

        '''
            这道题就是按着螺旋的结构，一步步的去fill all number in matrix
            那么这里的问题就在于如何的fill，这个是这道题的问题关键
            top = 0 # top layer: top row index
            right = n - 1 # right layer: right col index
            bottom = n - 1 # bottom layer: bottom row index
            left = 0 # left layer: left col index
         
        '''
        
        # 第一步要做的就是initial 一个matrix,将所有位置先填上数字0
        matrix = []
        # 在这个matrix里面，一共需要的行数是n，每一行都需要有一个单独的array，来存列数为n的数字
        # 所以第一个循环是一共有多少行，得创建多少个row_array
        for i in range(n):
            row_array = []  # 新建row_array
            # 这里就是在每一行都要有n个列，也就是说row_array里面都需要储存n个数字
            for j in range(n):
                # 所有有多少个n，就有多少个0要append进row_array 里面
                row_array.append(0)
            # 这里每一行创建完了，就都要储存到matrix里面
            matrix.append(row_array)


        # 第二步就开始往matrix里面螺旋状的存数字
        # layer by layer strategy
        num = 1
        top = 0 # top layer: top row index
        right = n - 1 # right layer: right col index
        bottom = n - 1 # bottom layer: bottom row index
        left = 0 # left layer: left col index

        # 这里的track的条件就是看有没有转到最中心的位置
        # 存的顺序就是存上面，右边，下边，左边
        while left <= right and top <= bottom:
            # 这里先top开始存数字
            # 这里right+1是因为，in range of 不包含最后的数字
            for i in range(left, right+1): 
                matrix[top][i] = num  # to fill top row, fix the top row index and increment the col position
                num += 1 # update num
            top += 1 # 这里top的index是往下面走，所以是+1

            # 存完上面的开始存右边的
            for i in range(top,bottom+1):
                matrix[i][right] = num
                num += 1
            right -= 1 # 这里的right的index是往里面缩，所以是-1

            # 然后转到bottom的一行
            # 注意这里是rever的order，所以需要从
            for i in range(right,left-1,-1): 
                matrix[bottom][i] = num
                num += 1
            bottom -= 1#这里的bottom是往上缩小的，所以也是-1
            
            #最后这里转到的是左边的
            for i in range(bottom,top-1,-1):
                matrix[i][left] = num
                num += 1
            left += 1 #这里左边的存完，是往右边缩，所以是+1
            
        return matrix
```

