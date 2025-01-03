## [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return *the **maximum** profit you can achieve*.

 

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
```

**Example 2:**

```
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.
```

**Example 3:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.
```

 

**Constraints:**

- `1 <= prices.length <= 3 * 104`
- `0 <= prices[i] <= 104`

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        '''
        greedy
        只有找到了比当前大的了，就进行交易
        然后profit + 上
        '''
        res = 0
        
        for i in range(1,len(prices)):
            if prices[i] - prices[i-1] > 0:
                res += prices[i] - prices[i-1]
        return res
```



## [55. Jump Game](https://leetcode.com/problems/jump-game/)

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` *if you can reach the last index, or* `false` *otherwise*.

 

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 105`



```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:

        '''
        这道题需要返回的是看能不能到达最后
        nums里面的每个数字代表的就是可以走的步子，看能不能走到最后
        这道题可以从后往前走，如果后面能走到前面，那么就代表肯定能走到后面
        
        这里需要记录的是一个last index，从后面往前面check
        如果前面的数字加上他本身的index 是大于或者等于last index，说明能走到前面的数字
        (这里的数字代表的是他能走的步数，他自己的index代表是当前的位置)
        最后返回的就是last index能不能走到0
        O(n)
        O(1)
        '''

        # 记录最后数字的index
        last_index = len(nums) - 1
        # 这里从倒数第二个数字开始check
        for i in range(len(nums))[::-1]:
            # 这里就是倒数第二个数字的index+nums应该是要
            # 大于或者等于最后一个数字的，这样才能到达最后一个数字
            if nums[i] + i >= last_index:
                # 如果最后一个数字符合要求，update last_index
                last_index = i
                
        return last_index == 0
```



## [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)

You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:

- `0 <= j <= nums[i]` and
- `i + j < n`

Return *the minimum number of jumps to reach* `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

 

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [2,3,0,1,4]
Output: 2
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- It's guaranteed that you can reach `nums[n - 1]`.



```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        
        '''
        这道题需要返回的是用最少的步数，从头跳到尾
        这里guarantee是能走到最后的
        如果要用最小的步数，那么就是每一步都要走最多的步数
        '''
        
        '''
        这里的思路是要记录下来每一个能跳到的范围，还有当前范围里面能reach到的最远的点
        记录能跳的范围是为了记录是不是要开跳了，记录能reach到最远的点是为了minimum 步数
        '''
        #initial res记录需要跳的步子
        res = 0
        #这里记录一下当前jump的边界在哪里，farest记录的是最远能跳的范围
        curr_end,farest = 0,0
        
        #这里一个for loop一个数字一个数字的track
        #距离当前的范围能最远能跳的位置，如果i到了curr_end,就update curr_end为当前最远能跳的位置
        '''
        从0开始
        第一个字母就是jump start的点
        ex1： [2] -> [3,1] ->
        '''
        for i in range(len(nums)-1):
            
            farest = max(farest,i+nums[i])
            
            if i == curr_end:
                res += 1
                curr_end = farest
                
        return res
```



## [1005. Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/)

Given an integer array `nums` and an integer `k`, modify the array in the following way:

- choose an index `i` and replace `nums[i]` with `-nums[i]`.

You should apply this process exactly `k` times. You may choose the same index `i` multiple times.

Return *the largest possible sum of the array after modifying it in this way*.

 

**Example 1:**

```
Input: nums = [4,2,3], k = 1
Output: 5
Explanation: Choose index 1 and nums becomes [4,-2,3].
```

**Example 2:**

```
Input: nums = [3,-1,0,2], k = 3
Output: 6
Explanation: Choose indices (1, 2, 2) and nums becomes [3,1,0,2].
```

**Example 3:**

```
Input: nums = [2,-3,-1,5,-4], k = 2
Output: 13
Explanation: Choose indices (1, 4) and nums becomes [2,3,-1,5,4].
```

 

**Constraints:**

- `1 <= nums.length <= 104`

- `-100 <= nums[i] <= 100`

- `1 <= k <= 104`

  

```python
class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        
        for _ in range(k):
            smallest = min(nums)
            idx_small = nums.index(smallest)
            nums[idx_small] = -smallest
        return sum(nums)
```

