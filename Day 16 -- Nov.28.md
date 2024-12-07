## [513. Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/)

Given the `root` of a binary tree, return the leftmost value in the last row of the tree.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/14/tree1.jpg)

```
Input: root = [2,1,3]
Output: 1
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/14/tree2.jpg)

```
Input: root = [1,2,3,4,null,5,6,null,null,7]
Output: 7
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-231 <= Node.val <= 231 - 1`





```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        '''
        这里是要找到最下层的最左侧的node的值
        '''

        # bfs
        queue = deque()
        current = root
        queue.append(current)

        while queue:
            current = queue.popleft()

            if current.right:
                queue.append(current.right)

            if current.left:
                queue.append(current.left)

        return current.val


        stack = [root]
        for node in stack:
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        return node.val
```



## [112. Path Sum](https://leetcode.com/problems/path-sum/)

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf** path such that adding up all the values along the path equals `targetSum`.

A **leaf** is a node with no children.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: true
Explanation: The root-to-leaf path with the target sum is shown.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
Input: root = [1,2,3], targetSum = 5
Output: false
Explanation: There are two root-to-leaf paths in the tree:
(1 --> 2): The sum is 3.
(1 --> 3): The sum is 4.
There is no root-to-leaf path with sum = 5.
```

**Example 3:**

```
Input: root = [], targetSum = 0
Output: false
Explanation: Since the tree is empty, there are no root-to-leaf paths.
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        ##recursion
        #O(n)
        #O(n) since in the worst case, when the tree is completely unbalanced
        # But in the best case (the tree is balanced), the height of the tree would be log⁡(N)\log(N)log(N)
        if not root:
            return False

        targetSum -= root.val
        if not root.left and not root.right:  # if reach a leaf
            return targetSum == 0
        return self.hasPathSum(root.left, targetSum) or self.hasPathSum(root.right, targetSum)

##############################################################

        #这里就是track，每一条路子的所有的值是不是等于target
        #这里因为需要一个个往下面track，可以用dfs
        #iterative dfs
        
        #这里需要设置一个dfs的function
        def dfs( node, currsum):
            #如果不是node，return none
            if not node:
                return None
            
            #过一个 node，加一下当前node的value
            currsum = currsum + node.val
            
            #如果已经到底了，就代表这条path 到头了
            if not node.left and not node.right:
                return currsum == targetSum #就判断是不是跟target相等
            #如果还有其他的路子，就search其他的点，还有当前的加起来的值
            return dfs(node.left,currsum) or dfs(node.right,currsum)
        
        return dfs(root,0)

#########################################################################

        if not root:
            return False
        
        q = [(root, root.val)]  # 初始化队列，包含根节点及其值
        while q:
            node, v = q.pop(0)  # 使用队列实现BFS，弹出队列首元素
            
            if not node:
                continue

            # 检查当前节点是否是叶节点
            if not node.left and not node.right:
                if v == sum:
                    return True  # 如果是叶节点且路径和等于sum，返回True
                continue

            # 将左右子节点加入队列
            if node.left:
                q.append((node.left, node.left.val + v))
            if node.right:
                q.append((node.right, node.right.val + v))
        return False


################################################
'''
iterative dfs vs recursion dfs vs bfs

1. 时间复杂度
所有方法的时间复杂度都是O(n)，因为每个节点至少被访问一次。

2. 空间复杂度
recursion DFS: 最坏情况下的空间复杂度是O(n)，在高度平衡的树中是O(log n)。
iterative DFS: 空间复杂度类似于recursion DFS，因为栈的最大深度与树的高度有关。
BFS: 在最坏情况下，空间复杂度可能达到O(n)，尤其是在树的最后一层，如果节点数最多。

3. 适用场景
DFS: 更适合目标路径深度大或需要访问所有节点的问题。
BFS: 优于在需要最短路径或涉及层次的问题，如最短路径问题或层次遍历。

4. 实际应用
recursion DFS: 代码简洁，易于实现，但可能存在栈溢出的风险。
iterative DFS: 控制更灵活，避免了栈溢出的风险。
BFS: 保证了按层次顺序访问，适合宽度优先的探索需求，如层次遍历
'''
```



## [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/)

Given the `root` of a binary tree and an integer `targetSum`, return *all **root-to-leaf** paths where the sum of the node values in the path equals* `targetSum`*. Each path should be returned as a list of the node **values**, not node references*.

A **root-to-leaf** path is a path starting from the root and ending at any leaf node. A **leaf** is a node with no children.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
Explanation: There are two paths whose sum equals targetSum:
5 + 4 + 11 + 2 = 22
5 + 8 + 4 + 5 = 22
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
Input: root = [1,2,3], targetSum = 5
Output: []
```

**Example 3:**

```
Input: root = [1,2], targetSum = 0
Output: []
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`



```python
# 定义一个二叉树节点类
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution(object):
    def pathSum(self, root, targetSum):

        # O(N2) 
        #the time complexity for visiting all nodes is O(N)
        #calculating the sum for these paths can take up to O(D), maximum deepth
        #       worst case is skewed tree(linkedin list) O(N)

        #O(N), best case O(logn) complete balance tree
        #       O(n)store the path

        # 如果根节点为空，则直接返回空列表
        if not root:
            return []

        # 定义一个dfs函数，用来深度优先搜索所有从根到叶的路径
        def dfs(root, path):
            # 检查当前节点是否为叶节点
            if not root.left and not root.right:
                # 如果是叶节点，检查当前路径加上当前节点的值的总和是否等于目标值
                if targetSum == sum(path + [root.val]):
                    # 如果等于目标值，将该路径添加到结果列表中
                    return res.append(path + [root.val])
            
            # 如果当前节点有左子节点，递归调用dfs搜索左子树
            if root.left:
                dfs(root.left, path + [root.val])
            
            # 如果当前节点有右子节点，递归调用dfs搜索右子树
            if root.right:
                dfs(root.right, path + [root.val])

        # 初始化一个空列表，用于存储所有符合条件的路径
        res = []

        # 从根节点开始调用dfs函数
        dfs(root, [])

        # 返回包含所有符合条件的路径的列表
        return res

```









## [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

 

**Constraints:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` and `inorder` consist of **unique** values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is **guaranteed** to be the preorder traversal of the tree.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        '''
        这道题就是要根据两个list 构成一个bst
        preorder就是 node在前面， node left right
        inorder 就是 node在中间， left node right

        这里就是可以根据preorder 和inorder的root的情况，把整个tree分区处理

        用recursion
        '''

        # 有一个没有，input有问题，直接返回None
        if not preorder or not inorder:
            return None

        # edge case
        # 只有一个node
        if len(preorder) == 1:
            return TreeNode(preorder[0])

        # root就是preorder最前面的
        root_node = preorder[0]
        root_idx = inorder.index(root_node)
        root = TreeNode(root_node)

        inorder_left = inorder[:root_idx]
        inorder_right = inorder[(root_idx + 1):]

        # preorder可以用 root_idx作为分界点，是因为数量一样
        # 而inorder里面root_idx之前的左节点数量和preorder是一样的
        # 这里就是跳过idx 0， 取了root_idx 个元素， 后面不include，所以要加一
        preorder_left = preorder[1:root_idx+1]
        preorder_right = preorder[root_idx+1:]

        root.left = self.buildTree(preorder_left, inorder_left)
        root.right = self.buildTree(preorder_right, inorder_right)

        return root
```





## [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return *the binary tree*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: inorder = [-1], postorder = [-1]
Output: [-1]
```

 

**Constraints:**

- `1 <= inorder.length <= 3000`
- `postorder.length == inorder.length`
- `-3000 <= inorder[i], postorder[i] <= 3000`
- `inorder` and `postorder` consist of **unique** values.
- Each value of `postorder` also appears in `inorder`.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.
- `postorder` is **guaranteed** to be the postorder traversal of the tree.



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        
        '''
        这里需要inorder和postorder来建立tree
        这里只需要找到root就可以分割inorder into two parts

        postorder left right root
        inorder   left root right
        '''

        # recursion

        if not inorder or not postorder:
            return None

        node_val = postorder[-1]
        
        node_idx_inorder = inorder.index(node_val)
        inorder_left = inorder[:node_idx_inorder]
        inorder_right = inorder[node_idx_inorder+1:]

        # 这两的left是一样的位置和个数
        postorder.pop()
        postorder_left = postorder[:node_idx_inorder]
        postorder_right = postorder[node_idx_inorder:]

        node = TreeNode(node_val)
        node.left = self.buildTree(inorder_left, postorder_left)
        node.right = self.buildTree(inorder_right, postorder_right)

        return node
```

