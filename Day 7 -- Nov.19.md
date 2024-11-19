## [454. 4Sum II](https://leetcode.com/problems/4sum-ii/)

Given four integer arrays `nums1`, `nums2`, `nums3`, and `nums4` all of length `n`, return the number of tuples `(i, j, k, l)` such that:

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

 

**Example 1:**

```
Input: nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
Output: 2
Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

**Example 2:**

```
Input: nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
Output: 1
```

 

**Constraints:**

- `n == nums1.length`
- `n == nums2.length`
- `n == nums3.length`
- `n == nums4.length`
- `1 <= n <= 200`
- `-228 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 228`

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        '''
        这道题需要找的是有多少种可能把nums里面的数字加起来 等于0
        这里跟two sum的思路是一样的，
        这里可以把nums1和nums2记录在hashmap，然后根据nums3和nums4的和去找
        需要return的是总的tuple的数量
        '''
        
        #On2
        #On2
        cnt = 0

        m = collections.defaultdict(int)

        for a in nums1:
            for b in nums2:
                m[a + b] += 1

        # m = {-1:1,0:2,1:1}

        for c in nums3:
            for d in nums4:
                # 这里要找的是c+d的相反组合
                find = -(c+d)
                # 最终的cnt就是要找到有没有c+d组合的相反数字
                # 也就是和c+d加起来能等于0的数字
                cnt += m[find]

#         -(-1,1,2,4)
#         count = 2
                
        return cnt
```



## [383. Ransom Note](https://leetcode.com/problems/ransom-note/)

Given two strings `ransomNote` and `magazine`, return `true` *if* `ransomNote` *can be constructed by using the letters from* `magazine` *and* `false` *otherwise*.

Each letter in `magazine` can only be used once in `ransomNote`.

 

**Example 1:**

```
Input: ransomNote = "a", magazine = "b"
Output: false
```

**Example 2:**

```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```

**Example 3:**

```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```

 

**Constraints:**

- `1 <= ransomNote.length, magazine.length <= 105`
- `ransomNote` and `magazine` consist of lowercase English letters.

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        '''
         这道题给了两个string，一个是ransomNote，还有一个magazin
         这里要找的就是ransomNote里面的char是不是来自magazin
         而且magazin里面每一个字母只能用一次

         因为涉及到字母次数问题，可以用Counter直接拿到字母char和freq的对应
         然后一个个去比较字母
         如果ransomNote里面的字母不在magazin里面，或者是freq大于magazine
        '''

        ransom = Counter(ransomNote)
        mag = Counter(magazine)

        for key, val in ransom.items():
            if key not in mag.keys() or val > mag[key]:
                return False
        return True
```



## [15. 3Sum](https://leetcode.com/problems/3sum/)

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

 

**Example 1:**

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```

**Example 2:**

```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```

**Example 3:**

```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```

 

**Constraints:**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`



```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:

        '''
        这道题就是two sum plus，找到三个数字总和是0
        '''
        #brute force
        res = set()

        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                for k in range(j + 1, len(nums)):
                    if i != j and j != k and i != k and nums[i] + nums[j] + nums[k] == 0:
                        res.add(tuple(sorted((nums[i], nums[j], nums[k]))))
        return res

#################################################################################################

        '''
        思路：two sum + one more loop
        先loop一遍nums，对每一个对应的num找剩下nums里面的element有没有two sum
        '''
        # res设为set去重，dups检查是否走有数字一样的element，节省时间
        res, dups= set(), set()
        # 存two sum里对应的数字和位数
        # key: nums[j], val: i
        hashmap = {}

        for i in range(len(nums)):
            # 检查是否有重复数字
            if nums[i] not in dups:
                dups.add(nums[i])
                # 计算剩下的two sum的target
                target = 0 - nums[i]
                # 从当前的下一个element算起
                for j in range(i + 1, len(nums)):
                    diff = target - nums[j]
                    # 检查diff是否在hashmap里，同时检查是否在当前i存的，有的话证明sum是0
                    if diff in hashmap and hashmap[diff] == i:
                        res.add(tuple(sorted((nums[i], nums[j], diff))))
                    # 没有的话把对应的i的index存到对应的key里
                    hashmap[nums[j]] = i

        return res

#################################################################################################
        # tc:o(n2)
        # sc: o(n)
        res, dups = set(), set()
        seen = {}
        for i, val1 in enumerate(nums):
            if val1 not in dups:
                dups.add(val1)
                for j, val2 in enumerate(nums[i+1:]):
                    complement = -val1 - val2
                    if complement in seen and seen[complement] == i:
                        res.add(tuple(sorted((val1, val2, complement))))
                    seen[val2] = i
        return res
```



## [18. 4Sum](https://leetcode.com/problems/4sum/)

Given an array `nums` of `n` integers, return *an array of all the **unique** quadruplets* `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are **distinct**.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [1,0,-1,0,-2,2], target = 0
Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

**Example 2:**

```
Input: nums = [2,2,2,2,2], target = 8
Output: [[2,2,2,2]]
```

 

**Constraints:**

- `1 <= nums.length <= 200`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`



```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        '''
        这道题就是base在 two sum之上的, 套ksum
        '''
        
        #tc: O(n^(k-1))
        #sc: O(n)
        #recursion + twosum
        def kSum(nums: List[int], target: int, k: int) -> List[List[int]]:
            
            res = []

            #这里的三个if 全是edge cases
            if not nums:
                return res
            
            #这里的average value用来判断后面有没有数字 valid 加入到res里面
            average_value = target // k            
            if average_value < nums[0] or nums[-1] < average_value:
                return res
            #这里为了让2sum更快，直接call twosum
            if k == 2:
                return twoSum(nums, target)
        
                
            for i in range(len(nums)):  
                # 如果这里curr和之前的一样，那么直接跳过
                if i == 0 or nums[i - 1] != nums[i]:                    
                    for subset in kSum(nums[i + 1:], target - nums[i], k - 1):
                        res.append([nums[i]] + subset)    
            return res

    
        #这里two sum就是用第一题的 binary search
        def twoSum(nums: List[int], target: int) -> List[List[int]]:
            res = []
            left, right = 0, len(nums) - 1
    
            while (left < right):
                curr_sum = nums[left] + nums[right]
                # 如果cursum小于target，left指针大于0并且和上一个数字一样（为了去重），left加一
                if curr_sum < target or (left > 0 and nums[left] == nums[left - 1]):
                    left += 1
                # 如果cursum大于target，right指针大于0并且和上一个数字一样（为了去重），right加一
                elif curr_sum > target or (right < len(nums) - 1 and nums[right] == nums[right + 1]):
                    right -= 1
                else:
                    res.append([nums[left], nums[right]])
                    left += 1
                    right -= 1
                                                         
            return res

        #先sort，然后得到k个sum
        nums.sort()
        return kSum(nums, target, 4)
```

