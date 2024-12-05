## [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)

Given a binary tree, determine if it is 

**height-balanced**



**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

```
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false
```

**Example 3:**

```
Input: root = []
Output: true
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-104 <= Node.val <= 104`



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        
        ## recursion 
        # top down
        # 从头开始search，看是不是一样
        # O nlogn
        # O n

        def height(node):
            if not node:
                return -1
            return max(height(node.left), height(node.right)) + 1

        if not root:
            return True

        return (
            abs(height(root.left) - height(root.right)) < 2
            and self.isBalanced(root.left)
            and self.isBalanced(root.right)
        )


        ### bottom up
        # 从最底下往上面loop
        def helper(node):
            if not root:
                return True, -1
            
            left_balance, left_height = helper(node.left)
            if not left_balance:
                return False, 0
            
            right_balance, right_height = helper(node.right)
            if not right_balance:
                return False, 0

            return abs((left_height-right_height) < 2 , \
                    1+ max(left_height, right_height))

        return helper(root)[0]
```





## [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

Given the `root` of a binary tree, return *all root-to-leaf paths in **any order***.

A **leaf** is a node with no children.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
```

**Example 2:**

```
Input: root = [1]
Output: ["1"]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 100]`.
- `-100 <= Node.val <= 100`





```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        
        '''
        这里要找到tree的path，就是dfs 深度搜索
        '''

        # dfs recursion
        def dfs(node, path):

            if node:
                path += str(node.val)
                if not node.left and not node.right:
                    res.append(path)
                else:
                    dfs(node.left, path + "->")
                    dfs(node.right, path + "->")
        
        res = []
        dfs(root, "")
        return res

        # dfs iteration
        # stack

        res = []
        stack = [root, str(root.val)]

        while stack:
            node, path = stack.pop()
            if not node.left and not node.right:
                res.append(path)
            
            if node.left:
                stack.append(node.left, path + "->" + str(node.left.val))

            if node.right:
                stack.append(node.right, path + "->" + str(node.right.val))
        return res
```



## [222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and `2h` nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/14/complete.jpg)

```
Input: root = [1,2,3,4,5,6]
Output: 6
```

**Example 2:**

```
Input: root = []
Output: 0
```

**Example 3:**

```
Input: root = [1]
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5 * 104]`.
- `0 <= Node.val <= 5 * 104`
- The tree is guaranteed to be **complete**.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        
        '''
        找complete 树有多少个节点
        '''

        # O(n)

        ## 【优化】
        ## 检查树高，如果最左和最右一样高度
        ## 那么就是意味，直接返回树节点公式

        left = right = 0
        curr = root
        while curr:
            left += 1
            curr = curr.left
        
        curr= root
        while curr:
            right += 1
            curr = curr.right
        
        if left == right:
            return 2**left - 1

        return 1 + self.countNodes(root.left) + \
                self.countNodes(root.right) if root else 0
```





## [404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/)

Given the `root` of a binary tree, return *the sum of all left leaves.*

A **leaf** is a node with no children. A **left leaf** is a leaf that is the left child of another node.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/04/08/leftsum-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 24
Explanation: There are two left leaves in the binary tree, with values 9 and 15 respectively.
```

**Example 2:**

```
Input: root = [1]
Output: 0
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `-1000 <= Node.val <= 1000`



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        
        '''
        算整个树的节点val
        dfs recursion
        '''
        # 这里比较tricky的点就是只要算left的leaves的值
        需要一个判断是不是left
        def dfs(node, is_left):
            if not node:
                return 0

            if not node.left and not node.right \
                and is_left:
                return node.val
            else:
                0

            return dfs(node.left, True) + dfs(node.right, False)
        return dfs(root, False)


        ## bfs
        # 按层来search

        q = deque( [(root, False)] )
        res = 0

        while q:
            # 一层层的记录，所以这里是要loop这一层
            for i in range(len(q)):
                node, is_left = q.popleft()
                # 如果这个node是leaves 并且是这一层的最左边
                if not node.left and not node.right \
                    and is_left:
                    res += node.val
                if node.left:
                    q.append((node.left, True))
                if node.right:
                    q.append((node.right, False))
        return res


```

