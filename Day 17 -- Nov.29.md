## [654. Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)

You are given an integer array `nums` with no duplicates. A **maximum binary tree** can be built recursively from `nums` using the following algorithm:

1. Create a root node whose value is the maximum value in `nums`.
2. Recursively build the left subtree on the **subarray prefix** to the **left** of the maximum value.
3. Recursively build the right subtree on the **subarray suffix** to the **right** of the maximum value.

Return *the **maximum binary tree** built from* `nums`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

```
Input: nums = [3,2,1,6,0,5]
Output: [6,3,5,null,2,0,null,null,1]
Explanation: The recursive calls are as follow:
- The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
    - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
        - Empty array, so no child.
        - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].
            - Empty array, so no child.
            - Only one element, so child is a node with value 1.
    - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
        - Only one element, so child is a node with value 0.
        - Empty array, so no child.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/24/tree2.jpg)

```
Input: nums = [3,2,1]
Output: [3,null,2,null,1]
```

 

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 1000`
- All integers in `nums` are **unique**.



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        
        '''
        找到最大值，设置成root，然后update nums
        O n^2  On
        '''
        if not nums:
            return

        node_val = max(nums)
        node_idx = nums.index(node_val)
        left_part = nums[:node_idx]
        right_part = nums[node_idx+1:]

        nums.remove(node_val)

        node = TreeNode(node_val)
        node.left = self.constructMaximumBinaryTree(left_part)
        node.right = self.constructMaximumBinaryTree(right_part)
        
        return node

        ###########################
        # 这里有个优化就是可以用stack
        # 因为思路就是 大的数字是node，两边小的就是左右
        # 那么就是对于3来说，他比后面数字大，所以2应该在他的右边child
        #            对于2来说，他比后面的数字大，所以1是他的右边child
        #             然后把1和2都pop出来，发现6还是比三大，所有

        stack = []
        for n in nums:
            node = TreeNode(n)

            while stack and stack[-1].val < n:
                node.left = stack.pop()
            
            if stack:
                stack[-1].right = node
            
            stack.append(node)
        return stack[0]
```





## [617. Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/)

You are given two binary trees `root1` and `root2`.

Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.

Return *the merged tree*.

**Note:** The merging process must start from the root nodes of both trees.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/05/merge.jpg)

```
Input: root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
Output: [3,4,5,5,4,null,7]
```

**Example 2:**

```
Input: root1 = [1], root2 = [1,2]
Output: [2,2]
```

 

**Constraints:**

- The number of nodes in both trees is in the range `[0, 2000]`.
- `-104 <= Node.val <= 104`



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:

        # recursion 
        # Om Om
        if not root1:
            return root2
        
        if not root2:
            return root1
        
        root_val = root1.val + root2.val
        node = TreeNode(root_val)
        node.left = self.mergeTrees(root1.left, root2.left)
        node.right = self.mergeTrees(root1.right, root2.right)
        return node

        '''
        bfs
        
        难点在于deque得存三个root，两个是需要check的root，还有一个是新建的root
        '''
        if not root1: 
            return root2
        if not root2: 
            return root1
        
        new_root = TreeNode()
        queue = deque([(root1, root2, new_root)])
        
        while queue:
            n1, n2, n = queue.popleft()
            if n1: 
                n.val = n1.val
            if n2: 
                n.val += n2.val

            if (n1 and n1.left) or (n2 and n2.left):
                n.left = TreeNode()
                queue.append((n1.left if n1 else None, n2.left if n2 else None, n.left))
            if (n1 and n1.right) or (n2 and n2.right):
                n.right = TreeNode()
                queue.append((n1.right if n1 else None, n2.right if n2 else None, n.right))

        return new_root
```







## [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)

You are given the `root` of a binary search tree (BST) and an integer `val`.

Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/12/tree1.jpg)

```
Input: root = [4,2,7,1,3], val = 2
Output: [2,1,3]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/01/12/tree2.jpg)

```
Input: root = [4,2,7,1,3], val = 5
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 5000]`.
- `1 <= Node.val <= 107`
- `root` is a binary search tree.
- `1 <= val <= 107`





```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        
        # dfs On
        if not root:
            return 
        
        if root.val == val:
            return root
        
        return self.searchBST(root.left,val) or \
                self.searchBST(root.right,val)

        # bfs
        q = deque([root])
        while q:
            for _ in range(len(q)):
                node = q.popleft()
                if node.val == val:
                    return node
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
        return


        ####### optimize binary search
        # Ologn
        # 不用全部search，只比较大小search
        # 因为binary search tree left < root < right
        while root:
            
            if root.val==val:
                return root
            
            elif val < root.val:
                root=root.left
                
            else:
                root=root.right
```





## [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.

A **valid BST** is defined as follows:

- The left

   

  subtree

   

  of a node contains only nodes with keys

   

  less than

   

  the node's key.

- The right subtree of a node contains only nodes with keys **greater than** the node's key.

- Both the left and right subtrees must also be binary search trees.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
Input: root = [2,1,3]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-231 <= Node.val <= 231 - 1`



```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def isValidBST(self, root):
        #这里就是要求node的value在left和right之间，达成 node.left<node<node.right
        #那么这里要做的就是对比left，node，和right这三个数字
        
        #initial mini是-inf， max是inf
        return self.dfs(root, float('-inf'), float('inf')) #注意这里的顺序，和return left和right的顺序
        
        #       Node
        #      /    \
        #   mini     max
        #left： -inf < mini < Node
        #right: Node < max < inf
    
    def dfs(self,node,mini,maxm):
        if not node:
            return True
            
        if node.val <= mini or node.val >= maxm:
            return False
            
            #那么这里return的是第二层的check，如果第一个if statement里面，node.val在 min-max的范围里，那么接下来返回的就是下一层的node，check
            # mini < node.left < node.val
        #left =  self.dfs(node.right,node.val,maxm) #所以在这里return的就是node.left的左半边
        #node.val < node.right < maxm
        #right = self.dfs(node.right,node.val,maxm) 
        
        return self.dfs(node.left, mini,node.val) and self.dfs(node.right,node.val,maxm) 
```

