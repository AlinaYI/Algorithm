## [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

Given the `root` of a binary search tree and the lowest and highest boundaries as `low` and `high`, trim the tree so that all its elements lies in `[low, high]`. Trimming the tree should **not** change the relative structure of the elements that will remain in the tree (i.e., any node's descendant should remain a descendant). It can be proven that there is a **unique answer**.

Return *the root of the trimmed binary search tree*. Note that the root may change depending on the given bounds.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/09/09/trim1.jpg)

```
Input: root = [1,0,2], low = 1, high = 2
Output: [1,null,2]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/09/09/trim2.jpg)

```
Input: root = [3,0,4,null,2,null,null,1], low = 1, high = 3
Output: [3,2,null,1]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `0 <= Node.val <= 104`
- The value of each node in the tree is **unique**.
- `root` is guaranteed to be a valid binary search tree.
- `0 <= low <= high <= 104`

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        '''
        recursion
        On On
        '''
        def trim(node):
            if not node:
                return None
            elif node.val > high:
                return trim(node.left)
            elif node.val < low:
                return trim(node.right)
            else:
                node.left = trim(node.left)
                node.right = trim(node.right)
                return node

        return trim(root)
```







## [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

Given an integer array `nums` where the elements are sorted in **ascending order**, convert *it to a* 

***height-balanced\***

 *binary search tree*.



 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg)

```
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/02/18/btree.jpg)

```
Input: nums = [1,3]
Output: [3,1]
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` is sorted in a **strictly increasing** order.



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        '''
        这里要build tree，然后顺序是左中右
        '''
        if not nums:
            return

        mid = (len(nums)-1)//2

        node = TreeNode(nums[mid])
        node.left = self.sortedArrayToBST(nums[:mid])
        node.right = self.sortedArrayToBST(nums[mid+1:])

        return node
```





## [538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/)

Given the `root` of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

As a reminder, a *binary search tree* is a tree that satisfies these constraints:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/05/02/tree.png)

```
Input: root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
Output: [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

**Example 2:**

```
Input: root = [0,null,1]
Output: [1,null,1]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-104 <= Node.val <= 104`
- All the values in the tree are **unique**.
- `root` is guaranteed to be a valid binary search tree.



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        '''
        这里search的顺序应该是右中左，postorder
        因为这里需要的是把比当前node大的所有的val加起来，所以是从大往小的search
        '''
        def dfs(node, val):
            if not node:
                return val
            node.val += dfs(node.right, val)
            return dfs(node.left, node.val)

        sum = dfs(root, 0)
        return root
```





Binary Tree

解题的最重要几个步骤：

1. 理解题目，想清楚是怎么遍历的

2. recursion 想清楚base case 和recursion case

   ```
   def rec(node):
   	# base
   	if not node:
   		return 
   	
   	# 这里的recursion要看具体题目
   	rec()
   ```



​	dfs 用stack，注意pop的顺序，要注意题目的要求

​	bfs 用deque，注意存的是每一行node，所以要loop的是len(q)

3. 要注意bst 和 普通tree的区别
