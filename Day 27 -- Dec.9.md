# 贪心算法

贪心算法就是，每一步都是最优解法

也就是当前情况下最好的办法，也就是贪心算法

## 贪心一般解题步骤

贪心算法一般分为如下四步：

- 将问题分解为若干个子问题
- 找出适合的贪心策略
- 求解每一个子问题的最优解
- 将局部最优解堆叠成全局最优解

这个四步其实过于理论化了，我们平时在做贪心类的题目时，如果按照这四步去思考，真是有点“鸡肋”。

做题的时候，只要想清楚 局部最优 是什么，如果推导出全局最优，其实就够了。



## [455. Assign Cookies](https://leetcode.com/problems/assign-cookies/)

Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child `i` has a greed factor `g[i]`, which is the minimum size of a cookie that the child will be content with; and each cookie `j` has a size `s[j]`. If `s[j] >= g[i]`, we can assign the cookie `j` to the child `i`, and the child `i` will be content. Your goal is to maximize the number of your content children and output the maximum number.

 

**Example 1:**

```
Input: g = [1,2,3], s = [1,1]
Output: 1
Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
```

**Example 2:**

```
Input: g = [1,2], s = [1,2,3]
Output: 2
Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 
You have 3 cookies and their sizes are big enough to gratify all of the children, 
You need to output 2.
```

 

**Constraints:**

- `1 <= g.length <= 3 * 104`
- `0 <= s.length <= 3 * 104`
- `1 <= g[i], s[j] <= 231 - 1`



```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        '''
        这道题就正常写，
        久因为给的children和cookies都是incresing order
        这里就是greedy，一个个往上面加
        '''

        g.sort() # children greed factor
        s.sort() # cookie size

        child_pointer = 0
        cookie_pointer = 0
        while child_pointer < len(g) and cookie_pointer < len(s):
            if g[child_pointer] <= s[cookie_pointer]:
                child_pointer += 1
            cookie_pointer += 1

        return child_pointer
```



## [376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/)

A **wiggle sequence** is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with one element and a sequence with two non-equal elements are trivially wiggle sequences.

- For example, `[1, 7, 4, 9, 2, 5]` is a **wiggle sequence** because the differences `(6, -3, 5, -7, 3)` alternate between positive and negative.
- In contrast, `[1, 4, 7, 2, 5]` and `[1, 7, 4, 5, 5]` are not wiggle sequences. The first is not because its first two differences are positive, and the second is not because its last difference is zero.

A **subsequence** is obtained by deleting some elements (possibly zero) from the original sequence, leaving the remaining elements in their original order.

Given an integer array `nums`, return *the length of the longest **wiggle subsequence** of* `nums`.

 

**Example 1:**

```
Input: nums = [1,7,4,9,2,5]
Output: 6
Explanation: The entire sequence is a wiggle sequence with differences (6, -3, 5, -7, 3).
```

**Example 2:**

```
Input: nums = [1,17,5,10,13,15,10,5,16,8]
Output: 7
Explanation: There are several subsequences that achieve this length.
One is [1, 17, 10, 13, 10, 16, 8] with differences (16, -7, 3, -3, 6, -8).
```

**Example 3:**

```
Input: nums = [1,2,3,4,5,6,7,8,9]
Output: 2
```

 

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 1000`



```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        
        '''
        这道题就是要看有多少substring
        有符合要求的， res += 1
        不符合要求，就skip到下一个数字，查符不符合要求
        '''
        # initial value = 1是因为无论第一个数字是正是负数都可以
        # 所以最开始的len肯定是1
        # 这里记录两个res的值，一个是整数的diff，一个是负数的diff
        positive_res = 1
        negative_res = 1

        for i in range(1, len(nums)):

            if nums[i] - nums[i-1] > 0:
                positive_res = negative_res+1
            elif nums[i] - nums[i-1] < 0:
                negative_res = positive_res+1
        return max(negative_res, positive_res)
```



## [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Given an integer array `nums`, find the subarray with the largest sum, and return *its sum*.

**Example 1:**

```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum 6.
```

**Example 2:**

```
Input: nums = [1]
Output: 1
Explanation: The subarray [1] has the largest sum 1.
```

**Example 3:**

```
Input: nums = [5,4,-1,7,8]
Output: 23
Explanation: The subarray [5,4,-1,7,8] has the largest sum 23.
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`



```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
#         #initial dp是原数组
#         #dp里面存的是 0-i 的subarry summ 或者是他自己本身
#         #如果自己本身的数字比前面的summ要大，前面就有负数，没有必要存着，直接update
#         #最后return的就是dp里面最大
#         dp = nums.copy()

#         for i in range(1,len(nums)):
            
#             dp[i] = max(dp[i],dp[i-1]+nums[i])

#         return max(dp)
    
        #这道题需要找最大的sum，但是没有规定subarray的大小。并且只要求返回最大的sum就行。 那么就需要两个值来记录，一个是当前的sum，一个是最大的sum
        #最大的sum也就是需要返回的最终的res。cursum就是当前sum，用来和之前最大的sum对比
        #这里假定第一个是最大的，然后往后加，如果数字小于0，直接reset cursum为0，相当于跳过这个，开启下一个subarray
        #O(n)
        
        maxsub = nums[0]
        cursum = 0
        
        #nums里面的数字一个个check过去
        for n in nums:
            #如果大于0，就加入到cur sum里面，因为这里没有限制subarray的大小，随便加多少数字都行
            cursum += n
            
            #update max                
            maxsub = max(maxsub,cursum)
            
             #如果小于0，就reset cursum
            if cursum < 0:
                cursum = 0
            
        return maxsub
```

