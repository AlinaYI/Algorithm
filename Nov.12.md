# 数组理论基础

数组，指的是Array。 **Array是指存放在连续内存空间的相同类型的集合**

<u>**特点**</u>

- **数组下标都是从0开始的。**
- **数组内存空间的地址是连续的**

| language   | Array                                                        | List                                                         |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| C/C++      | 1. fixed size<br />2. same type, 必须存相同类型的数据<br />3. 内存地址是连续的<br />没有内置边界检查，高效但是容易越界 | 1. c 没有list，但是可以用linked list来实现类似list的功能<br />2. c++ 提供了 `std::vector`, `std::list`<br /> -- `std::vector`: 动态数组，可以随机访问，元素在内存中连续储存的<br />-- `std::list`: 双向链表，元素不连续，适合频繁的插入和删除，但是不支持随机访问 |
| Java       | 1. Array在Java中是一个object<br />2. fixed size<br />3. same type, 必须存相同类型的数据 | 1. Java 提供了内置的 List 接口以及常用的实现类，如 `ArrayList` 和 `LinkedList`：<br/>-- `ArrayList`：基于动态数组实现，支持快速随机访问，适合频繁读取数据的场景。<br/>-- `LinkedList`：基于双向链表实现，适合频繁插入和删除的场景。<br/>2. List 是 Java 集合框架（Java Collections Framework）的一部分，位于 `java.util` 包中 |
| JavaScript | 1. Dynamic size<br />2. 可以存any type of data               | 没有list数据结构，但是可以用Array 替代                       |
| Python     | 没有array，如果要用，那么就要用到array module -- Numpy<br />一般python中会用list来代替array | 1. Dynamic 数组<br />2. 可以存any type of data<br />3. 支持切片操作，list[1:2] |
| C#         | 1. fixed size<br />2. same type, 必须存相同类型的数据        | 1. C# 提供了 `List<T>` 类，用于存储泛型元素，是 `System.Collections.Generic` 命名空间的一部分<br />2. `List<T>` 是基于动态数组的实现，支持自动调整大小。 |



Python 本身没有内置的数组类型，但可以使用 `list` 作为数组的替代，或者使用 `array` 模块。



# [704. Binary Search](https://leetcode.com/problems/binary-search/)

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```

**Example 2:**

```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 < nums[i], target < 104`
- All the integers in `nums` are **unique**.
- `nums` is sorted in ascending order.



```Python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        '''
        	思路就是一个个查，直到查到target值
        '''
         #brute force
         # tc : O(n)  sc: O(1)
       
         for i in range(len(nums)):
             if nums[i] == target:
                 return i
         return -1
    
    


class Solution:
    def search(self, nums: List[int], target: int) -> int:
        '''
        经典二分，
        注意：
        1. 这里二分的查找一般是需要数组是sorted的
        2. left和right的取值范围，和返回的值
        
        tc O(logn) sc O(1)
        '''
    
        left,right = 0, len(nums)-1
        
        while right >= left:
            
            mid = left + (right-left)//2
            
            # 找到了就返回
            if nums[mid] == target:
                return mid
            # 如果中间的值大于target的，那么就找小的那部分
            elif nums[mid] > target:
                right = mid - 1
            # 如果中间的值小于target的，那么就找小的那部分
            elif nums[mid] < target:
                left = mid + 1
        return -1
        
    
```



# [27. Remove Element](https://leetcode.com/problems/remove-element/)

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm). The order of the elements may be changed. Then return *the number of elements in* `nums` *which are not equal to* `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

**Custom Judge:**

The judge will test your solution with the following code:

```
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

If all assertions pass, then your solution will be **accepted**.

 

**Example 1:**

```
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Example 2:**

```
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
```



```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        '''
        two pointer
        tc：O(n) sc：O(1)
        
        这道题要in place remove所有和val相等的数字，不考虑后面的数字和顺序
        用两个pointer从左和右往中间check比较
        如果left pointer对应的数字不和val相等，就不管，left向右移动
        如果left pointer对应的数字和val相同，证明要remove，就和right对应的不等于val的数字交换
        如果right是等于val，就向左移
        最后当left和right相等，证明已经走完了，return left，是最后array的长度
        '''
        
        left, right = 0, len(nums) - 1
        print(left, right)
        
        while left <= right:
            # 如果left不等于val，left往后移动
            if nums[left] != val:
                left += 1
            
            # 如果left等于val，证明需要remove，和right对应的数字对换
            else:
                if nums[right] != val:
                    nums[left] = nums[right]
                right -= 1
                    
        return left
    
    
    	'''
    		这里是对binary search的一个代码的优化，
    		用了快慢指针，fast遍历每一个element，slow指向的下一个要赋予的值
    		虽然这里的tc是一样的，但是因为不需要频繁的交换element
    		在一些情况下，是会稍稍的提升一点性能
    		tc:O(n) sc:O(1)
    	'''
    	slow = 0
        for fast in range(len(nums)):
            #如果当前的fast不是val，那么就把值换给slow
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
        return slow
```





# [977. Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)

Given an integer array `nums` sorted in **non-decreasing** order, return *an array of **the squares of each number** sorted in non-decreasing order*.

 

**Example 1:**

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```

**Example 2:**

```
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` is sorted in **non-decreasing** order.

 

**Follow up:** Squaring each element and sorting the new array is very trivial, could you find an `O(n)` solution using a different approach?

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        
        
         '''
         brute force
         就是一个个的process，算出平方，然后排序
         这里排序就是logn
         O(n) --> loop数组平方 + O(nlogn) -->sort
         tc：O(nlogn)	
         '''
        
         for i in range(len(nums)):
            
             nums[i]  = nums[i]*nums[i]
        
         # 这里nums.sort() 用的是原地排序，不需要额外的空间
         # 那么这里的 sc就是O(1)
         # nums.sort()
         # return nums
            
         # 使用了sorted函数
         # 这个函数会创建一个新list存结果，需要额外的空间
         # 所以这里sc就是O(n)
         return sorted(nums)
        
          

        '''
        	因为这里的数据是按照non-decreasing order，
        	也就是数组可能是一样的或者是increasing order
        	那么因为这里的nums可能是负数，这道题要算的是平方
        	平方的话，肯定是个正数，所以这里可以从两边取数字比较
            那么就是这里可以建立一个res array，
            从两边的absolute value的去比较，然后填充整个res
            那么就可以通过比较绝对值把整个数组sort好，不用再额外排序
        '''
    	#tc O(n) sc:O(n)
    	
        # 建一个res的数组，存最后的答案
        res = [-1]*len(nums)
        left = 0
        right = len(nums)-1

        for i in range(len(nums)-1,-1,-1):
            
            if abs(nums[left]) < abs(nums[right]):
                temp = nums[right]
                right -= 1
            else:
                temp = nums[left]
                left += 1

            res[i] = temp*temp

        return res
```

