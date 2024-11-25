# Stack and Queue

### **Stack (栈)**

- **栈** 是一种 **后进先出（LIFO: Last In, First Out）** 的数据结构。

#### 操作：

1. **Push**: 往栈顶添加元素。
2. **Pop**: 从栈顶移除元素。
3. **Peek/Top**: 查看栈顶的元素，但不移除。
4. **IsEmpty**: 判断栈是否为空。

#### 应用场景：

- 函数调用（递归使用的调用栈）。
- 括号匹配问题（如检查表达式的括号是否有效）。
- 深度优先搜索（DFS）。

#### 各语言实现：

1. Python: 使用 `list` 

   ```Python
   pythonCopy codestack = []  # 使用 list
   stack.append(1)  # Push
   stack.pop()      # Pop
   ```

2. Java: 使用 `java.util.Stack `

   ```java
   javaCopy codeStack<Integer> stack = new Stack<>();
   stack.push(1);  // Push
   stack.pop();    // Pop
   ```

3. C++: 使用 `std::stack`

   ```C++
   cppCopy codestd::stack<int> stack;
   stack.push(1);  // Push
   stack.pop();    // Pop
   ```

4. JavaScript: 使用 `Array`

   ```javascript
   javascriptCopy codelet stack = [];
   stack.push(1);  // Push
   stack.pop();    // Pop
   ```



### **Queue (队列)**

- **队列** 是一种 **先进先出（FIFO: First In, First Out）** 的数据结构。

#### 操作：

1. **Enqueue**: 往队尾添加元素。
2. **Dequeue**: 从队头移除元素。
3. **Peek/Front**: 查看队头的元素，但不移除。
4. **IsEmpty**: 判断队列是否为空。

#### 应用场景：

- 任务调度（如 CPU 任务队列）。
- 广度优先搜索（BFS）。
- 消息队列（Message Queue）。

#### 各语言实现：

1. Python: 使用 `collections.deque` 或 `queue.Queue`

   ```Python
   pythonCopy codefrom collections import deque
   queue = deque()
   queue.append(1)   # Enqueue
   queue.popleft()   # Dequeue
   ```

2. Java : 使用 `java.util.Queue` 接口（如 `LinkedList` 或 `ArrayDeque`）

   ```java
   javaCopy codeQueue<Integer> queue = new LinkedList<>();
   queue.add(1);  // Enqueue
   queue.poll();  // Dequeue
   ```

3. C++: 使用 `std::queue`

   ```c++
   cppCopy codestd::queue<int> queue;
   queue.push(1);  // Enqueue
   queue.pop();    // Dequeue
   ```

4. JavaScript: 使用 `Array`或自定义队列。

   ```javascript
   javascriptCopy codelet queue = [];
   queue.push(1);      // Enqueue
   queue.shift();      // Dequeue
   ```





# [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

 

**Example 1:**

```
Input
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 1, 1, false]

Explanation
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

 

**Constraints:**

- `1 <= x <= 9`
- At most `100` calls will be made to `push`, `pop`, `peek`, and `empty`.
- All the calls to `pop` and `peek` are valid.

 

**Follow-up:** Can you implement the queue such that each operation is **[amortized](https://en.wikipedia.org/wiki/Amortized_analysis)** `O(1)` time complexity? In other words, performing `n` operations will take overall `O(n)` time even if one of those operations may take longer.



```python
'''
这道题是要用stack来实现一个queue，
那么 queue 和 stack最大的区别就是 queue FIFO，而 stack是FILO
所以要实现queue需要用两个stack来写
一个是主要存数据的，一个是用来辅助的。这样能保证最开始的数据存在最后面
这样可以直接pop last element，如果用stack的pop(0)的话，是On， 这里就是O1
'''
class MyQueue:

    def __init__(self):
        # 这里s2用来辅助s1
        # push element的时候需要一个list 暂存数据
        self.s1 = []
        self.s2 = []

    def push(self, x: int) -> None:
        
        # 如果right_order里面有element，先移动到reversed里面
        # 这样在存进right_order的时候就是能push进最前面
        # 后进来的数据就在最前面，实现FIFO
        while self.s1:
            self.s2.append(self.s1.pop())
        self.s1.append(x)
        while self.s2:
            self.s1.append(self.s2.pop())

    def pop(self) -> int:
        # 因为push的时候就调整了顺序，所以这里直接pop 最后一个element
        # 就是最开始存的element
        return self.s1.pop()

    def peek(self) -> int:
        # 这里peek是要看最早的element
        # 那么在这里就是list中最后一个element
        return self.s1[-1]

    def empty(self) -> bool:
        return not self.s1


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```





# [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

- `void push(int x)` Pushes element x to the top of the stack.
- `int pop()` Removes the element on the top of the stack and returns it.
- `int top()` Returns the element on the top of the stack.
- `boolean empty()` Returns `true` if the stack is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a queue, which means that only `push to back`, `peek/pop from front`, `size` and `is empty` operations are valid.
- Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.

 

**Example 1:**

```
Input
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 2, 2, false]

Explanation
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // return 2
myStack.pop(); // return 2
myStack.empty(); // return False
```



```python
'''
这道题跟232是相反的，也就是用queue来实现stack
queue是FIFO，stack是FILO
'''

class MyStack:

    def __init__(self):
        self.q = deque()

    def push(self, x: int) -> None:
        # O(n)
        # 因为要实现stack中的FILO
        # 那么就要把最先进queue里面的element 移动到queue的队末
        # 因为对于q来说，是先pop头
        self.q.append(x)
        for _ in range(len(self.q) - 1):
            self.q.append(self.q.popleft())

    def pop(self) -> int:
        # O(1)
        # 这样子队头就是新加的元素了
        # 这里可以直接pop
        return self.q.popleft()

    def top(self) -> int:
        # O(1)
        return self.q[0]

    def empty(self) -> bool:
        # O(1)
        return len(self.q) == 0

# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```





# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

 

**Example 1:**

**Input:** s = "()"

**Output:** true

**Example 2:**

**Input:** s = "()[]{}"

**Output:** true

**Example 3:**

**Input:** s = "(]"

**Output:** false

**Example 4:**

**Input:** s = "([])"

**Output:** true

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.



```python
class Solution:
    def isValid(self, s: str) -> bool:
        '''
        这是一道经典的stack题目
        遇到open bracket就append进stack，
        遇到close的时候就pop
        这样能把每一对open close都匹配上
        '''

        # 因为要用close匹配open，所以这里diction里面存的是close
        brackets = {")": "(", "}": "{", "]": "["}

        # 建立stack
        stack = []
        # s中一个个loop char
        for char in s:
            # 如果char在brackets中，也就是意味着遇到close括号了
            if char in brackets:
                # 如果stack里面有东西，并且stack中最后的一个element能匹配上
                # 就pop最后一个element，说明找到匹配的一对了
                if stack and stack[-1] == brackets[char]:
                    stack.pop()
                # 如果stack是empty或者是匹配不上，说明是错的
                else:
                    return False
            # 如果找到的不在brackets里面，说明还没有找到close的括号
            # 那么就先把当前的open 括号存到stack里面
            else:
                stack.append(char)
        return True if not stack else False
```





# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

You are given a string `s` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.

We repeatedly make **duplicate removals** on `s` until we no longer can.

Return *the final string after all such duplicate removals have been made*. It can be proven that the answer is **unique**.

 

**Example 1:**

```
Input: s = "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
```

**Example 2:**

```
Input: s = "azxxzy"
Output: "ay"
```

 

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of lowercase English letters.

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        '''
        这道题的意思就是中间碰上了相同的就消除
        如果一样的消除了之后，又碰上了一样的也要消除
        直到不一样了
        那么这里的思路就是stack
        
        每次存数字进栈的时候看下和上一个字母是不是一样的
            如果是一样的，那么就pop
            如果不一样，存进栈
        '''

        stack = []

        for char in s:
            if stack and char == stack[-1]:
                stack.pop()
            else:
                stack.append(char)
        return "".join(stack)
```

