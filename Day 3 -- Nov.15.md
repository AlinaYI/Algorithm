# 链表

##  链表基础知识

Type of linear data structure. 通过节点串联在一起，每一个节点有两个部分，一个数据域，一个是指针域。

链表是通过指针域的指针链接在内存中各个节点。所以链表中的节点在内存中不是连续分布的 ，而是**散乱分布在内存中的某地址上**，分配机制取决于操作系统的内存管理。



链表分为：

- 单链表

![链表1](https://camo.githubusercontent.com/829520739bac1ba45419058035cb0c6174bf996c98b8f15d795741f540597125/68747470733a2f2f636f64652d7468696e6b696e672d313235333835353039332e66696c652e6d7971636c6f75642e636f6d2f706963732f32303230303830363139343532393831352e706e67)

- 双链表 :  

​		双链表：每一个节点有两个指针域，一个指向下一个节点，一个指向上一个节点。

双链表 既可以向前查询也可以向后查询。

![链表2](https://camo.githubusercontent.com/5fa11a1ed756d7041d5566b05863b2e9db1bb97476823ae5df32a714713af1ab/68747470733a2f2f636f64652d7468696e6b696e672d313235333835353039332e66696c652e6d7971636c6f75642e636f6d2f706963732f32303230303830363139343535393331372e706e67)

- 循环链表

​		循环链表，顾名思义，就是链表首尾相连。

​		循环链表可以用来解决约瑟夫环问题。

![链表4](https://camo.githubusercontent.com/2b41ca195ae0a57b2e0b9fee108b0b418e28e7fa0be9ca4525047defbdc648d6/68747470733a2f2f636f64652d7468696e6b696e672d313235333835353039332e66696c652e6d7971636c6f75642e636f6d2f706963732f32303230303830363139343632393630332e706e67)



```python
class ListNode:
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
```



## [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return *the new head*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg)

```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```

**Example 2:**

```
Input: head = [], val = 1
Output: []
```

**Example 3:**

```
Input: head = [7,7,7,7], val = 7
Output: []
```

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 104]`.
- `1 <= Node.val <= 50`
- `0 <= val <= 50`

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        
        '''
        None -> 1 -> 2 -> 6 -> 3 -> 4 -> 5 -> 6 -> None
        prev   curr
        4种情况：
            1. 删的是最前面的一个，head -> head.next
            2. 删的是中间的一个， pre -> curr.next
            3. 删的是最后一个，pre-> none
            4. 没有数字需要删除， iterate 步骤

            **整个linked-list是空的
        '''
        # O(n), O(1)

        prev = None
        curr = head

        while curr:
            if curr.val == val:
                # 是不是第一个要删除的
                # if prev != None
                # 不是删的开头
                # prev -> curr -> curr.next
                if prev:
                    prev.next = curr.next
                # 是第一个要删除的node
                # 如果是要删的头，那么直接移动head
                else:
                    head = curr.next

            # 如果不是要找的val
            # update prev
            else:
                prev = curr
            
            # 因为不止删除一个，万一后面还有要删掉的
            # 所以这里每次都要update curr
            # 直到走到最后面
            curr = curr.next
        return head
```



## [707. Design Linked List](https://leetcode.com/problems/design-linked-list/)

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.
A node in a singly linked list should have two attributes: `val` and `next`. `val` is the value of the current node, and `next` is a pointer/reference to the next node.
If you want to use the doubly linked list, you will need one more attribute `prev` to indicate the previous node in the linked list. Assume all nodes in the linked list are **0-indexed**.

Implement the `MyLinkedList` class:

- `MyLinkedList()` Initializes the `MyLinkedList` object.
- `int get(int index)` Get the value of the `indexth` node in the linked list. If the index is invalid, return `-1`.
- `void addAtHead(int val)` Add a node of value `val` before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
- `void addAtTail(int val)` Append a node of value `val` as the last element of the linked list.
- `void addAtIndex(int index, int val)` Add a node of value `val` before the `indexth` node in the linked list. If `index` equals the length of the linked list, the node will be appended to the end of the linked list. If `index` is greater than the length, the node **will not be inserted**.
- `void deleteAtIndex(int index)` Delete the `indexth` node in the linked list, if the index is valid.

 

**Example 1:**

```
Input
["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
[[], [1], [3], [1, 2], [1], [1], [1]]
Output
[null, null, null, null, 2, null, 3]

Explanation
MyLinkedList myLinkedList = new MyLinkedList();
myLinkedList.addAtHead(1);
myLinkedList.addAtTail(3);
myLinkedList.addAtIndex(1, 2);    // linked list becomes 1->2->3
myLinkedList.get(1);              // return 2
myLinkedList.deleteAtIndex(1);    // now the linked list is 1->3
myLinkedList.get(1);              // return 3
```

 

**Constraints:**

- `0 <= index, val <= 1000`
- Please do not use the built-in LinkedList library.
- At most `2000` calls will be made to `get`, `addAtHead`, `addAtTail`, `addAtIndex` and `deleteAtIndex`.



两种solution，第一种可以用list

```python
class MyLinkedList:

    def __init__(self):
        self.my_list = []

    def get(self, index: int) -> int:
        if len(self.my_list) > index:
            return self.my_list[index]
        else:
            return -1
    def addAtHead(self, val: int) -> None:
        self.my_list.insert(0, val)

    def addAtTail(self, val: int) -> None:
        self.my_list.append(val)

    def addAtIndex(self, index: int, val: int) -> None:
        if len(self.my_list) > index:
            self.my_list.insert(index, val)
        elif len(self.my_list) == index:
            self.my_list.append(val)
        else:
            pass
    def deleteAtIndex(self, index: int) -> None:
        if len(self.my_list) > index:
            self.my_list.pop(index)
        else:
            pass


# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)



```



第二种用node，组成double linked list

```python
class Node:
    def __init__(self,val):
        self.val = val
        self.prev = None
        self.next = None

class MyLinkedList:

    def __init__(self):
        
        self.head = Node(0)
        self.tail = Node(0)
        self.head.next = self.tail
        self.tail.prev = self.head
        
        self.size = 0
        
        '''
             <-
        head -> tail
        '''
        
    def get(self, index: int) -> int:
        
        #edge case
        if index < 0 or index >= self.size:
            return -1
        
        #从前面开始search
        if index + 1 < self.size - index:
            node = self.head
            for _ in range(index + 1):
                node = node.next
        #从后开始search   
        else:
            
            node = self.tail
            for _ in range(self.size - index):
                node = node.prev
                
        return node.val
    
    
    def addAtHead(self, val: int) -> None:
        
        '''
        新建一个node， node加入到我们的linked list
        
           ->       <-
        pre<- node -> nxt
        '''
        node = Node(val)
        
        pre= self.head
        nxt = self.head.next
        
        '''
                  <-
        pre  node -> nxt
        '''
        nxt.prev = node
        node.next = nxt
        
        '''
          ->       <-
        pre<- node -> nxt
        
        '''
        pre.next = node
        node.prev = pre
        
        
        self.size += 1

        
    def addAtTail(self, val: int) -> None:
        
        '''
        新建一个node， node加入到我们的linked list
        
                  ->
        pre  node <- nxt
        '''
        
        node = Node(val)
        
        pre = self.tail.prev
        nxt = self.tail
        
        '''
                  ->
        pre  node <- nxt
        '''
        nxt.prev = node
        node.next = nxt
        
        
        '''
            ->    ->
        pre <- node <- nxt
        '''
        pre.next = node
        node.prev = pre
        
        
        self.size += 1
        

    def addAtIndex(self, index: int, val: int) -> None:
        '''
        新建一个node， node加入到我们的linked list
        找到index在的位置
        '''
        
        node = Node(val)
        
        #edge case
        if index > self.size:
            return        
        # if index < 0:
        #     index = 0
        
        '''
        size = 8
        insert = 7
                    8
        1,2,3,4,5,6,7,8
        8,7,6,5,4,3,2,1
        '''
        
        '''从后或者是从前找index前一位的loop,找到insert位置的前一位，和后一位'''
        if index + 1 < self.size - index:            
            #从前往后search
            pre = self.head
            
            for _ in range(index):                
                pre = pre.next
            nxt = pre.next
        
        else:
            #从后往前search
            nxt = self.tail
            for _ in range(self.size - index):
                nxt = nxt.prev
            pre = nxt.prev
            
        
        '''
                 ->
        pre node <- nxt
        '''
        
        nxt.prev = node
        node.next = nxt        
        
        '''
            ->      ->
        pre <-  node <- nxt
        '''
        
        node.prev = pre
        pre.next = node
        
            
        self.size += 1
        
    def deleteAtIndex(self, index: int) -> None:
        
        #edge case
        if index <0 or index >= self.size:
            return 
        
        #从前面开始search要删除的pre和nxt
        if index + 1 < self.size - index:
            node = self.head
            for _ in range(index + 1):
                node = node.next
        #从后面开始search
        else:
            node = self.tail
            for _ in range(self.size - index):
                node = node.prev
                
        #找到要删除得到node之后，进行删除 
         
            #pre -> node -> nxt
        
        if node.prev and node.next:
            pre = node.prev
            nxt = node.next
            
            pre.next = nxt
            nxt.prev = pre
            
            
        self.size -= 1

        
# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```



## [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
Input: head = [1,2]
Output: [2,1]
```

**Example 3:**

```
Input: head = []
Output: []
```

 

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        '''
        iteration Time O(n) Space O(1)
        先建一个空的pre node作为暂时的next
        对每个node都先把它的next存下来，然后把当前的node的next指向pre
        然后update pre为当前的node
        然后update head为next
        一直循环到没有下一个node
        '''

        # 临时的空node
        pre = None
        
        while head:
            # 现存head的next，这个是之前的下一个node
            nex = head.next
            # 然后改掉当前next指向的node
            head.next = pre
            # update
            pre = head
            head = nex

        # 最后返回pre因为head已经是None了，要返回前一个node
        return pre

##########################################################################
        '''
        recursive
        Time O(n) Space O(n)
        '''

        # base case 检查head和headl.next都不为None
        if (not head) or (not head.next):
            return head

        # recersively反转到最后的node
        pre = self.reverseList(head.next)
        # 将下一个node的next指向当前node
        head.next.next = head
        head.next = None

        return pre

##################################################################
        
        # 用helper function和pre
        pre = None
        
        def backtracking(node, pre):
            if not node:
                return pre
            # 存当前node本来的next
            nex = node.next
            node.next = pre
            pre = node
            return backtracking(nex, node)
        
        pre = backtracking(head, pre)
        
        return pre
```

