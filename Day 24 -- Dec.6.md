## [93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)

A **valid IP address** consists of exactly four integers separated by single dots. Each integer is between `0` and `255` (**inclusive**) and cannot have leading zeros.

- For example, `"0.1.2.201"` and `"192.168.1.1"` are **valid** IP addresses, but `"0.011.255.245"`, `"192.168.1.312"` and `"192.168@1.1"` are **invalid** IP addresses.

Given a string `s` containing only digits, return *all possible valid IP addresses that can be formed by inserting dots into* `s`. You are **not** allowed to reorder or remove any digits in `s`. You may return the valid IP addresses in **any** order.

 

**Example 1:**

```
Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]
```

**Example 2:**

```
Input: s = "0000"
Output: ["0.0.0.0"]
```

**Example 3:**

```
Input: s = "101023"
Output: ["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

 

**Constraints:**

- `1 <= s.length <= 20`
- `s` consists of digits only.



```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        '''

        这道题需要写的是一个valid ip address

        一个valid 有几点要求， 
        1：一列数字一共有三个点，也就是四组数字
        2：每一组数字都必须在 0-255 之间
        3：每一组数字不能包含除数字以外的符号
        4：每一组数字不能是0开头

        需要返回的是所有vaild组合
        '''

        # 这里就是要多一个check
        def is_valid(string):

            if len(string) > 1 and string[0] == '0':
                return False

            # 字符串不能为空，且转换为整数要在 0-255 范围内
            if not string or int(string) > 255:
                return False
            
            return True
        
        def backtrack(start, comb):

            if len(comb) > 4:
                return

            # return 条件就是reach the end, 并且其中只有三个点
            if start == len(s) and len(comb) == 4:
                res.append(".".join(comb))
                return
            
            for i in range(start, len(s)):
                part = s[start:i+1]
                if is_valid(part):
                    comb.append(part)
                    backtrack(i+1, comb)
                    comb.pop()
        res = []
        backtrack(0, [])
        return res
```





## [78. Subsets](https://leetcode.com/problems/subsets/)

Given an integer array `nums` of **unique** elements, return *all possible* *subsets* *(the power set)*.



The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

 

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**Example 2:**

```
Input: nums = [0]
Output: [[],[0]]
```

 

**Constraints:**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- All the numbers of `nums` are **unique**.



```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        '''
        这道题是要找出一个list所有的subset
        '''
        
        def backtrack(start, comb):
            res.append(comb.copy())
            
            for i in range(start, len(nums)):
             
                comb.append(nums[i])
                backtrack(i+1, comb)
                comb.pop()
                
        
        res = []
        backtrack(0,[])
        return res
```





## [90. Subsets II](https://leetcode.com/problems/subsets-ii/)

Given an integer array `nums` that may contain duplicates, return *all possible* 

*subsets* *(the power set)*.



The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

 

**Example 1:**

```
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**Example 2:**

```
Input: nums = [0]
Output: [[],[0]]
```

 

**Constraints:**

- `1 <= nums.length <= 10`

- `-10 <= nums[i] <= 10`

  

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        
        
        def backtrack(start, comb):
            # 这里就查一下重复
            if comb not in res:
                res.append(comb[:])
            else:
                return
            
            for i in range(start, len(nums)):
             
                comb.append(nums[i])
                backtrack(i+1, comb)
                comb.pop()
                
        
        res = []
        nums.sort()
        backtrack(0,[])
        return res
```

