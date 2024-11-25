# Binary Tree

refer：https://jokinkuang.github.io/2017/01/30/complete-binary-tree.html

https://github.com/youngyangyang04/leetcode-master/blob/master/problems/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.md

### 结点与叶子结点

结点就是树的一个节点，叶子结点是指度为0的结点，即没有分支的结点。
![tree-node](https://jokinkuang.github.io/w3c/images/algorithm/tree/tree-node.jpg)

### 深度与高度与度

对于**某个结点**，`深度`是指结点从**根结点**到自己的层数（根结点深度为1），`高度`是指结点所在的最长路径的**叶子结点**到自己的层数（叶子结点高度为1），`度`是指结点的儿子分支数。

对于**整棵树**，最深的叶子结点的深度是`树的深度`，树根的高度是`树的高度`，结点中最大的度是`树的度`。（可见，整棵树的深度和高度是相等的）。

![tree-deep-height-degree](https://jokinkuang.github.io/w3c/images/algorithm/tree/tree-deep-height-degree.jpg)
如图：
根结点：深度为1，高度为4，度为2。
A 结点：深度为2，高度为3，度为2。
B 结点：深度为4，高度为1，度为0。
整棵树：深度为4，高度为4，度为2。

小结：

1. 空树，深度是1，根结点的度是0度。
2. 像上面的满二叉树，深度是2，根结点的度是2度，两个叶结点的度是0度。
3. 结点的深度和高度不一定相同，很容易理解，深度是从上而下，高度是自底向上。
4. 整棵树而言，深度和高度相等。





二叉树有两种主要的形式：**满二叉树和完全二叉树。**

## 满二叉树（Full Binary Tree）

顾名思义，满二叉树是每个结点都有两个儿子的特殊的二叉树。
![full-binary-tree](https://jokinkuang.github.io/w3c/images/algorithm/tree/full-binary-tree.jpg)

满二叉树：深度为`n`，则有`2^n - 1`个结点，深度为`n`的层，则有`2^(n - 1)`个结点。



## 完美二叉树（Binary tree）

如果二叉树除了最后一层有缺失外，其它是满的，且最后一层缺失的叶子结点只出现在右侧，则这样的二叉树叫完全二叉树。定义是：若二叉树的深度为n，除第n层外，其余各层的结点都达到了最大，且第n层的结点都连续集中在最左边。简而言之，就是从左到右结点是连续不断的二叉树就叫完全二叉树。**满二叉树是特殊的完全二叉树**。

![complete-binary-tree](https://jokinkuang.github.io/w3c/images/algorithm/tree/complete-binary-tree.jpg)

完全二叉树：

1. 完全二叉树`只有`最后一层`右侧`可能出现缺失。
2. 度为1的结点最多有一个（二叉树结点的度最多为2，缺1个就是1度，缺2个就是0度）。
3. 右子树的深度为h，则左子树的深度必为h或h+1（完全二叉树只可能右侧缺失）。

完全二叉树规律：

1. 深度(高度)为`n`的完全二叉树，最多有`2^n - 1`个结点，因为最多是满二叉树。
2. 反过来，如果完全二叉树有`N`个结点，那么深度（高度）最多为`log(2)N`，（能简写为logN吗？）。（证明：2^n-1 = N）。
3. 完全二叉树的层数 = 深度 - 1，即``
4. 深度为`n`的层，最多有`2^(n-1)`个结点（和上面很相似）。（证明：每一层是上一层的2倍）。
5. 总结点数为`N`，深度为`n`的结点，其高度为`h = log(2)N - n + 1`。（深度为1的结点，高度等于树的高度；高度为1的结点，深度等于树的深度）。

像下面这样，从左到右，没有跳过结点，能够连续串起来的二叉树就是完全二叉树：
![complete-binary-tree-check](https://jokinkuang.github.io/w3c/images/algorithm/tree/complete-binary-tree-check.jpg)

下面是一些非完全二叉树：
![none-complete-binary-tree](https://jokinkuang.github.io/w3c/images/algorithm/tree/none-complete-binary-tree.jpg)



## 二叉树的遍历方式

二叉树主要有两种遍历方式：

1. 深度优先遍历：先往深走，遇到叶子节点再往回走。
2. 广度优先遍历：一层一层的去遍历。

**这两种遍历是图论中最基本的两种遍历方式**，后面在介绍图论的时候 还会介绍到。

那么从深度优先遍历和广度优先遍历进一步拓展，才有如下遍历方式：

- 深度优先遍历

  - 前序遍历（递归法，迭代法）中左右
  - 中序遍历（递归法，迭代法）左中右
  - 后序遍历（递归法，迭代法）左右中

  这里前中后，可以看成是中间node的顺序，看中间的node在哪里，就是什么顺序

- 广度优先遍历

  - 层次遍历（迭代法）



## 二叉树的定义

```python
class TreeNode:
    def __init__(self, val, left = None, right = None):
        self.val = val
        self.left = left
        self.right = right
```





# Preorder

## [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

Given the `root` of a binary tree, return *the preorder traversal of its nodes' values*.

**Example 1:**

**Input:** root = [1,null,2,3]

**Output:** [1,2,3]

**Explanation:**

![img](https://assets.leetcode.com/uploads/2024/08/29/screenshot-2024-08-29-202743.png)

**Example 2:**

**Input:** root = [1,2,3,4,5,null,8,null,null,6,7,9]

**Output:** [1,2,4,5,6,7,3,8,9]

**Explanation:**

![img](https://assets.leetcode.com/uploads/2024/08/29/tree_2.png)

**Example 3:**

**Input:** root = []

**Output:** []

**Example 4:**

**Input:** root = [1]

**Output:** [1]

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.

- `-100 <= Node.val <= 100`

  

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []

        left = self.preorderTraversal(root.left)
        right = self.preorderTraversal(root.right)

        return [root.val] + left + right

###############################################################
### iteration

        stack = [root]
        res = []

        while stack:
            node = stack.pop()
            if node is not None:
                res.append(node.val)
                if node.right:
                    stack.append(node.right)
                if node.left:
                    stack.append(node.left)

        return res
```



# Inorder

## [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

Given the `root` of a binary tree, return *the inorder traversal of its nodes' values*.

 

**Example 1:**

**Input:** root = [1,null,2,3]

**Output:** [1,3,2]

**Explanation:**

![img](https://assets.leetcode.com/uploads/2024/08/29/screenshot-2024-08-29-202743.png)

**Example 2:**

**Input:** root = [1,2,3,4,5,null,8,null,null,6,7,9]

**Output:** [4,2,6,5,7,1,3,9,8]

**Explanation:**

![img](https://assets.leetcode.com/uploads/2024/08/29/tree_2.png)

**Example 3:**

**Input:** root = []

**Output:** []

**Example 4:**

**Input:** root = [1]

**Output:** [1]

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        ##中序
        if not root:
            return []

        left = self.inorderTraversal(root.left)
        right = self.inorderTraversal(root.right)

        return left + [root.val] + right

###################################
#### Iteration
        res = [ ]
        stack = [ ]
        cur = root
        
        while cur or stack:
            while cur:
                stack.append(cur)
                cur = cur.left
            cur = stack.pop()
            res.append(cur.val)
            cur = cur.right
        return res

```





# Postorder

## [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

Given the `root` of a binary tree, return *the postorder traversal of its nodes' values*.

 

**Example 1:**

**Input:** root = [1,null,2,3]

**Output:** [3,2,1]

**Explanation:**

![img](https://assets.leetcode.com/uploads/2024/08/29/screenshot-2024-08-29-202743.png)

**Example 2:**

**Input:** root = [1,2,3,4,5,null,8,null,null,6,7,9]

**Output:** [4,6,7,5,2,9,8,3,1]

**Explanation:**

![img](https://assets.leetcode.com/uploads/2024/08/29/tree_2.png)

**Example 3:**

**Input:** root = []

**Output:** []

**Example 4:**

**Input:** root = [1]

**Output:** [1]

 

**Constraints:**

- The number of the nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100`



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:

        # recursion
        if not root:
            return []

        left = self.postorderTraversal(root.left)
        right = self.postorderTraversal(root.right)

        return left + right + [root.val]

        # iterative
        # preorder倒过来
        if not root:
            return []

        stack = [root]
        res = []

        while stack:
            node = stack.pop()
            res.append(node.val)
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)
        return res[::-1]
```



# 模板



## 顺序遍历

### 迭代法前序遍历：

```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        st= []
        if root:
            st.append(root)
        while st:
            node = st.pop()
            if node != None:
                if node.right: #右
                    st.append(node.right)
                if node.left: #左
                    st.append(node.left)
                st.append(node) #中
                st.append(None)
            else:
                node = st.pop()
                result.append(node.val)
        return result
```



### 迭代法中序遍历：

```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        st = []
        if root:
            st.append(root)
        while st:
            node = st.pop()
            if node != None:
                if node.right: #添加右节点（空节点不入栈）
                    st.append(node.right)
                
                st.append(node) #添加中节点
                st.append(None) #中节点访问过，但是还没有处理，加入空节点做为标记。
                
                if node.left: #添加左节点（空节点不入栈）
                    st.append(node.left)
            else: #只有遇到空节点的时候，才将下一个节点放进结果集
                node = st.pop() #重新取出栈中元素
                result.append(node.val) #加入到结果集
        return result
```



### 迭代法后序遍历：

```python
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        result = []
        st = []
        if root:
            st.append(root)
        while st:
            node = st.pop()
            if node != None:
                st.append(node) #中
                st.append(None)
                
                if node.right: #右
                    st.append(node.right)
                if node.left: #左
                    st.append(node.left)
            else:
                node = st.pop()
                result.append(node.val)
        return result
```



## 层序遍历

```python
# 利用长度法
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        queue = collections.deque([root])
        result = []
        while queue:
            level = []
            for _ in range(len(queue)):
                cur = queue.popleft()
                level.append(cur.val)
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            result.append(level)
        return result
```



```python
#递归法
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []

        levels = []

        def traverse(node, level):
            if not node:
                return

            if len(levels) == level:
                levels.append([])

            levels[level].append(node.val)
            traverse(node.left, level + 1)
            traverse(node.right, level + 1)

        traverse(root, 0)
        return levels
```
