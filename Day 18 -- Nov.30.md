## [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

Given the `root` of a Binary Search Tree (BST), return *the minimum absolute difference between the values of any two different nodes in the tree*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)

```
Input: root = [4,2,6,1,3]
Output: 1
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg)

```
Input: root = [1,0,48,null,null,12,49]
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[2, 104]`.
- `0 <= Node.val <= 105`



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        
        '''
        #recursion
        这里是bst，也就是 left < node < right
        那么就是最小的diff就是在parent和他的children里面

        所以这里可以记录之前的node，也就是parent
        然后记录当前的node-parent的diff
        
        这里用的是inorder，也就是left-> node -> right
        On On
        '''
        
        # 这里得设置成None，因为第一个deff是root和root.left
        # 如果是0的话，如果之后的diff都大，但是没有value是0的node
        # 就会影响最终的结果
        self.prevNode = None
        self.res = float("inf")

        def dfs(node):
            if not node:
                return

            # 先查node.left
            dfs(node.left)

            if self.prevNode is not None:
                self.res = min(self.res, node.val - self.prevNode)

            self.prevNode = node.val
            dfs(node.right)
        dfs(root)
        return self.res


        ###################################################
        ## 还可以用inorder的方法遍历tree，然后记录下来
        ## 因为bst，所以只要按顺序比较相邻的node

        inorder_nodes = []

        def inorder(node):
            if not node:
                return
            inorder(node.left)
            inorder_nodes.append(node.val)
            inorder(node.right)

        inorder(root)
        res = float("inf")
        for i in range(1,len(inorder_nodes)):
            res = min(res, inorder_nodes[i]-inorder_nodes[i-1])
        return res
```





## [501. Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/)

Given the `root` of a binary search tree (BST) with duplicates, return *all the [mode(s)](https://en.wikipedia.org/wiki/Mode_(statistics)) (i.e., the most frequently occurred element) in it*.

If the tree has more than one mode, return them in **any order**.

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys **less than or equal to** the node's key.
- The right subtree of a node contains only nodes with keys **greater than or equal to** the node's key.
- Both the left and right subtrees must also be binary search trees.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/11/mode-tree.jpg)

```
Input: root = [1,null,2,2]
Output: [2]
```

**Example 2:**

```
Input: root = [0]
Output: [0]
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-105 <= Node.val <= 105`



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        # 所有node存list
        # 然后用counter
        # On On 
        self.nodes = []
        def dfs(node):
            if not node:
                return

            self.nodes.append(node.val)
            dfs(node.left)
            dfs(node.right)

        dfs(root)
        count = Counter(self.nodes)
        max_freq = max(count.values())
        res = [key for key,value in count.items() if value == max_freq]
        return res

#############################################
## 优化的思路就是不适用额外的space
## On O1
        max_streak = 0
        curr_streak = 0
        curr_num = 0
        ans = []
        
        curr = root
        while curr:
            if curr.left:
                # Find the friend
                friend = curr.left
                while friend.right:
                    friend = friend.right
                
                friend.right = curr
                
                # Delete the edge after using it
                left = curr.left
                curr.left = None
                curr = left
            else:
                # Handle the current node
                num = curr.val
                if num == curr_num:
                    curr_streak += 1
                else:
                    curr_streak = 1
                    curr_num = num

                if curr_streak > max_streak:
                    ans = []
                    max_streak = curr_streak

                if curr_streak == max_streak:
                    ans.append(num)
                
                curr = curr.right
        
        return ans
```



## [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**

```
Input: root = [1,2], p = 1, q = 2
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[2, 105]`.
- `-109 <= Node.val <= 109`
- All `Node.val` are **unique**.
- `p != q`
- `p` and `q` will exist in the tree.



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        
        ''' 
        time: O(n)
        
        思路：preorder traversal dfs
        
        大概的逻辑就是需要找到p和q
        p q有可能在三个地方
        1.cur就是p或者q
        2.p, q中一个可能在cur的左边
        3.p, q中一个可能在cur的右边
        
        三个情况中又两个达成了就可以return结果了
        1.也就是说如果cur自己是q或者p并且其left_subtree或者right_subtree是的话，就return cur
        2.如果cur的left_subtree有p或q，并且right_subtree有p和q，就return cur
        
        implementation
        #base case如果tree走到头也没找到p，q，就return None
        #然后检查root是不是p或q，如果是的话就root
        1.先查q or q在不在left_subtree, 如果在的话return left
        2.然后查q or p在不在right_subtree，如果在的话就return right
        3.如果left and right --> 也就是如果p或者q在left 和 right里面都找到了,就return root
        
        
        然后返回给上一层是true还是false
        '''
        
        #initial basic
        if not root:
            return None
        
        #这里需要return的是node，也就是找到了这个node，那么就返回找到的这个root
        if root == p or root == q:
            return root
        
        #这里left 就朝着root.left search 
        #right 就朝着 root.right search
        left = self.lowestCommonAncestor(root.left,p,q)
        right = self.lowestCommonAncestor(root.right,q,p)
        
        #如果left 和 right 都search到 p，q，就是return root
        #这里就是左边也找到了一个node，右边也找到了一个node。那么直接return 
        if left and right:
            return root
        #只有一边search到，那么就说明右边没有想要的node。就return 那一边
        elif left:
            return left
        elif right:
            return right
```

