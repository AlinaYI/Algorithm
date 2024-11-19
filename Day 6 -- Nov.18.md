# 哈希表理论基础 

哈希表是根据关键码的值而直接进行访问的数据结构。

![哈希表1](https://camo.githubusercontent.com/e162a14c0cb17c5a2fb4b321b8489c040503216127b1cd21d185884934dffa55/68747470733a2f2f636f64652d7468696e6b696e672d313235333835353039332e66696c652e6d7971636c6f75642e636f6d2f706963732f32303231303130343233343830353136382e706e67)

| 操作 | 平均时间复杂度 | 最坏时间复杂度 |
| :--: | :------------: | :------------: |
| 查找 |      O(1)      |      O(n)      |
| 插入 |      O(1)      |      O(n)      |
| 删除 |      O(1)      |      O(n)      |



**<u>三种哈希结构</u>**

当我们想使用哈希法来解决问题的时候，我们一般会选择如下三种数据结构。

- 数组
- set （集合）
- map(映射)

这里数组就没啥可说的了，我们来看一下set。

在C++中，set 和 map 分别提供以下三种数据结构，其底层实现以及优劣如下表所示：

| 集合                 | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| -------------------- | -------- | -------- | ---------------- | ------------ | -------- | -------- |
| `std::set`           | 红黑树   | 有序     | 否               | 否           | O(log n) | O(log n) |
| `std::multiset`      | 红黑树   | 有序     | 是               | 否           | O(log n) | O(log n) |
| `std::unordered_set` | 哈希表   | 无序     | 否               | 否           | O(1)     | O(1)     |

std::unordered_set底层实现为哈希表，`std::set` 和`std::multiset`的底层实现是红黑树，红黑树是一种平衡二叉搜索树，所以key值是有序的，但key不可以修改，改动key值会导致整棵树的错乱，所以只能删除和增加。

| 映射                 | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| -------------------- | -------- | -------- | ---------------- | ------------ | -------- | -------- |
| `std::map`           | 红黑树   | key有序  | key不可重复      | key不可修改  | O(log n) | O(log n) |
| `std::multimap`      | 红黑树   | key有序  | key可重复        | key不可修改  | O(log n) | O(log n) |
| `std::unordered_map` | 哈希表   | key无序  | key不可重复      | key不可修改  | O(1)     | O(1)     |



**<u>hash table 和 Dictionary 的区别</u>**

| 语言   | Hashtable                                                  | Dictionary                                              |
| ------ | ---------------------------------------------------------- | ------------------------------------------------------- |
| C++    | `std::unordered_map`                                       | `std::map`（基于红黑树）                                |
| Java   | `HashMap`, `Hashtable`, `ConcurrentHashMap`                | 通常直接使用 `HashMap`                                  |
| Python | 没有直接的 `Hashtable`（底层 `dict` 使用类似哈希表的结构） | `dict` 是内置类型，功能强大，支持动态扩容、多种操作等。 |
| C#     | `System.Collections.Hashtable`（老旧）                     | `Dictionary<TKey, TValue>`（推荐，基于哈希表实现）。    |



## [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)

Given two strings `s` and `t`, return `true` if `t` is an 

anagram

 of `s`, and `false` otherwise.



**Example 1:**

**Input:** s = "anagram", t = "nagaram"

**Output:** true

**Example 2:**

**Input:** s = "rat", t = "car"

**Output:** false

 

**Constraints:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` and `t` consist of lowercase English letters.



**Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        
        #using Counter
        #O(n) O(n)
        #如果两个string 的长度不一样，直接return false
        if len(s) != len(t):
            return False
        
        #把两个string都Counter出来，然后走一个for loop 对比
        counts = Counter(s)
        countt = Counter(t)

        for key,val in counts.items():
            if val != countt[key]:
                return False            
        return True
######################################################################

        '''可以直接return这两个list的Counter是不是相等的'''
        return Counter(s) == Counter(t) # 1
        return sorted(s) == sorted(t) # 2
        

######################################################################     
        #hashmap
        
        # 0. base case, if the lengths of s and t are different, return False
        if len(s) != len(t):
            return False
        
        # 1. set up a hashmap with the counts of the letters in s
        hashmap = {}
        
        for letter in s:
            hashmap[letter] = hashmap.get(letter, 0) + 1
            
        # 2. check if the letters in t are in the hashmap, if not return False, remove once the letters cancel out
        for letter in t:
            if letter in hashmap:
                hashmap[letter] -= 1
                if hashmap[letter] == 0:
                    hashmap.pop(letter)
            else:
                return False
        # 3. return if the hashmap is blank
        return hashmap == {}
```





## [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)

Given two integer arrays `nums1` and `nums2`, return *an array of their* 

*intersection*

Each element in the result must be **unique** and you may return the result in **any order**.



**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
```

 

**Constraints:**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`



```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:

        # Time complexity : O(n+m)
        # Space complexity : O(m+n)
        set1 = set(nums1)
        set2 = set(nums2)
        res = []

        for i in set2:
            if i in set1:
                res.append(i)
        return res

#####################################################################

        set1 = set(nums1)
        set2 = set(nums2)
        return list(set2 & set1)

#####################################################################   
		# tc: O(mlogm+nlogn) 因为排序
    	# sc: O(m+n)
        nums1 = sorted(nums1)
        nums2 = sorted(nums2)
        p1 = 0
        p2 = 0
        
        r = set()
        while p1 < len(nums1) and p2 < len(nums2):
            val1 = nums1[p1]
            val2 = nums2[p2]
            if val1 == val2:
                r.add(val1)
                p1 += 1
            elif val1 < val2:
                p1 +=1
            elif val1 > val2:
                p2 += 1
                
        return r
        
```



## [202. Happy Number](https://leetcode.com/problems/happy-number/)

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
- Those numbers for which this process **ends in 1** are happy.

Return `true` *if* `n` *is a happy number, and* `false` *if not*.

 

**Example 1:**

```
Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

**Example 2:**

```
Input: n = 2
Output: false
```



```python
class Solution:
    def isHappy(self, n: int) -> bool:
        
        '''
        主要是要检查有没有cycle
        第一种是用hashmap存之前出现过的组合，如果碰到出现过的证明cycle了，但这个要extra space
        第二种是slow fast，slow走一个，fast走两个，如果fast追上slow了证明进入cycle了
        '''
        
        # 获取下一个组合
        def get_next(number):
            total_sum = 0
            while number > 0:
                number, digit = divmod(number, 10)
                total_sum += digit ** 2
            return total_sum
    
        # initialize slow是n，然后fast是n的下一个组合
        slow_runner = n
        fast_runner = get_next(n)
        # 当fast是1证明是happy，当slow和fast重合，证明进入cycle或者结束了
        while fast_runner != 1 and slow_runner != fast_runner:
            slow_runner = get_next(slow_runner)
            fast_runner = get_next(get_next(fast_runner))
        
        # 最后如果fast是1，是happy，剩下的不是1
        return fast_runner == 1
         
```



## [1. Two Sum](https://leetcode.com/problems/two-sum/)

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly\* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

 

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```
Input: nums = [3,3], target = 6
Output: [0,1]
```

 

**Constraints:**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **Only one valid answer exists.**

 

**Follow-up:** Can you come up with an algorithm that is less than `O(n2)` time complexity?

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        #hashmap

        hashmap = {}

        for idx, n in enumerate(nums):
            if target - n in hashmap:
                return [hashmap[target - n], idx]
            else:
                hashmap[n] = idx
```

