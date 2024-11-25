# [150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

You are given an array of strings `tokens` that represents an arithmetic expression in a [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Evaluate the expression. Return *an integer that represents the value of the expression*.

**Note** that:

- The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
- Each operand may be an integer or another expression.
- The division between two integers always **truncates toward zero**.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in a reverse polish notation.
- The answer and all the intermediate calculations can be represented in a **32-bit** integer.

 

**Example 1:**

```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```

**Example 2:**

```
Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```

**Example 3:**

```
Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

 

**Constraints:**

- `1 <= tokens.length <= 104`
- `tokens[i]` is either an operator: `"+"`, `"-"`, `"*"`, or `"/"`, or an integer in the range `[-200, 200]`.



```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        '''
        这道题是要按照给的顺序去计算tokens
        遇到符号，就pop前两个数字，然后按照给的符号计算

        这里用stack，遇到不是digit就pop前两个数字，然后用符号计算
        计算完再存进去

        ###注意stack pop出来的顺序
        '''

        stack = []
        for char in tokens:
            if char in "+-*/":
                num2 = int(stack.pop())
                num1 = int(stack.pop())
                if char == "+":
                    temp = num1 + num2
                elif char == "-":
                    temp = num1 - num2
                elif char == "*":
                    temp = num1 * num2
                elif char == "/":
                    temp = num1 / num2
                stack.append(temp)
            else:
                stack.append(char)
        return int(stack.pop())
```





# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return *the max sliding window*.

 

**Example 1:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`



```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        '''
        这道题要求的是返回所有 长度为k的windows中的最大数字

        two pointers组成windows
        iniital最大的就是max(nums[:k])

        # 如果对每个windows取max值的话，会tle
        # max 是 On

        那么这里的优化就是可以先把每个windows内的最大数字记录下来
        这里记录的话需要记录index，因为要用对比index来看，当前这个max有没有再当前的window里面

        这里可以用deque，因为需要从前面和后面都可以pop，这样可以优化pop的时间
        '''

        # 这里q存的是index
        q = deque()
        res = []

        # 一开始处理一下最开始k个，initial一下deque
        for i in range(k):
            # 如果当前的值大于deque里面的值
            # 就pop出来deque里面的值
            while q and nums[i] >= nums[q[-1]]:
                q.pop()
            q.append(i)

        res.append(nums[q[0]])

        # 处理剩下的
        for i in range(k, len(nums)):
            # 如果现在q里面存的最大值的index，超出当前的窗口
            #  把q中最左边的值pop 掉
            if q and q[0] == i-k:
                q.popleft()

            # 如果当前的值大于deque里面的值，那么就把deque里面的pop掉
            while q and nums[i] >= nums[q[-1]]:
                q.pop()

            # 要不然就是直接加入deque，方便后面处理
            q.append(i)
            res.append(nums[q[0]])
        return res
```



# [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

Given an integer array `nums` and an integer `k`, return *the* `k` *most frequent elements*. You may return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `k` is in the range `[1, the number of unique elements in the array]`.
- It is **guaranteed** that the answer is **unique**.

 

**Follow up:** Your algorithm's time complexity must be better than `O(n log n)`, where n is the array's size.



```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        '''
        这道题要返回的是top k frequ element
        也就是要找到 freq排序前k个element
        '''
        # edge case
        if k == len(nums):
            return nums

        freq = Counter(nums)
        return heapq.nlargest(k, freq.keys(), key=freq.get)

        # freq.get(key) 等价于 freq[key]，
        # 但 get 更安全：如果键不存在，不会抛出异常，而是返回 None(或自定义默认值)
```





# 总结

1. 注意element pop的顺序
2. 注意怎么去用stack 和 deque，看具体题目要求和需要什么
3. 优化的角度就是判断FIFO还是FILO
