## [39. Combination Sum](https://leetcode.com/problems/combination-sum/)

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the 

frequency

 of at least one of the chosen numbers is different.



The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

 

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []
```

 

**Constraints:**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 40`





```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        
        def backtrack(start, comb):
            if sum(comb) == target:
                res.append(comb.copy())
                return
            
            if sum(comb) > target:
                return

            for i in range(start, len(candidates)):

                '''
                这里添加的剪枝，就是优化
                '''
                #这里是当前要加入的数字比 需要的数字大，直接break
                if candidates[i] > target - sum(comb):
                    break
                    
                comb.append(candidates[i])
                backtrack(i, comb)
                comb.pop()

        res = []
        candidates.sort()
        backtrack(0, [])

        return res
```





## [40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

 

**Example 1:**

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**Example 2:**

```
Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
[1,2,2],
[5]
]
```

 

**Constraints:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        def backtrack(start,comb):
            if sum(comb) == target and comb not in res:
                res.append(comb.copy())
                return
            
            if sum(comb) > target:
                return
            
            for i in range(start, len(candidates)):
                
                '''
                这里两个就是剪枝
                '''
                # Very important here! We don't use `i > 0` because we always want 
                # to count the first element in this recursive step even if it is the same 
                # as one before. To avoid overcounting, we just ignore the duplicates
                # after the first element.
                if i > start and candidates[i] == candidates[i - 1]:
                    continue
            
                #这里就是search到如果当前要加入到comb里面的数字如果比target都要大，没有必要往后search了
                if candidates[i] > target:
                    break


                comb.append(candidates[i])
                backtrack(i+1, comb)
                comb.pop()

        res = []
        # 排序有助于 优化搜索 和 实现剪枝
        candidates.sort()
        backtrack(0, [])
        return res
```





## [131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

Given a string `s`, partition `s` such that every 

substring

 of the partition is a 

**palindrome**

. Return *all possible palindrome partitioning of* `s`.



 

**Example 1:**

```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

**Example 2:**

```
Input: s = "a"
Output: [["a"]]
```

 

**Constraints:**

- `1 <= s.length <= 16`
- `s` contains only lowercase English letters.





```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:

        '''
        这道题需要把一个string分成一个个的palindrome

        这里储存条件就是当backtrack的点到达了最后的点就储存到最后的res
        '''

        def is_palindrome(string):
            return string == string[::-1]

        # 这里backtrack是整个combination，然后最后pointer到达len(s)才能存到res里面
        def backtrack(start, comb):

            # return/add to result condition 
            if start == len(s)+1:
                res.append(comb.copy())
                return 

            for i in range(start, len(s)+1):
                # 如果是panlindrome，就加入到comb里面
                if is_palindrome(s[start - 1:i]):
                    comb.append(s[start - 1:i])
                    backtrack(i+1, comb)
                    comb.pop()

        res = []
        backtrack(1,[])

        return res
```

