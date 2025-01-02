## [491. Non-decreasing Subsequences](https://leetcode.com/problems/non-decreasing-subsequences/)

Given an integer array `nums`, return *all the different possible non-decreasing subsequences of the given array with at least two elements*. You may return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [4,6,7,7]
Output: [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

**Example 2:**

```
Input: nums = [4,4,3,2,1]
Output: [[4,4]]
```

 

**Constraints:**

- `1 <= nums.length <= 15`
- `-100 <= nums[i] <= 100`



```python
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        
        def backtrack(start, comb):
            
            if len(comb) > 1:
                res.add(tuple(comb[:]))
            
            for i in range(start, len(nums)):
                if not comb or comb[-1] <= nums[i]:
                    comb.append(nums[i])
                    backtrack(i+1, comb)
                    comb.pop()
        
        res = set()
        backtrack(0,[])
        return list(res)
```





## [46. Permutations](https://leetcode.com/problems/permutations/)

Given an array `nums` of distinct integers, return all the possible 

permutations. You can return the answer in **any order**.



**Example 1:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Example 2:**

```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```

**Example 3:**

```
Input: nums = [1]
Output: [[1]]
```

 

**Constraints:**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- All the integers of `nums` are **unique**.



```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        
        def backtrack(start,comb):
            
            if len(comb) == len(nums):
                res.append(comb[:])
            
            for i in range(len(nums)):
                
                if nums[i] not in comb:
                    comb.append(nums[i])
                    backtrack(start,comb)
                    comb.pop()
                    
        res = []
        backtrack(0,[])
        return res
```





## [47. Permutations II](https://leetcode.com/problems/permutations-ii/)

Given a collection of numbers, `nums`, that might contain duplicates, return *all possible unique permutations **in any order**.*

 

**Example 1:**

```
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**Example 2:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

 

**Constraints:**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`



```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        
        '''
        这道题就是46的unique的方法
        
        这里因为有重复的，可以选择记录每一个数字的个数，然后按照数字的个数来backtrack。
        这里可以直接选择用conter，因为counter已经记录了数字的frequency。 然后这里append完要减去数字的frequent，然后pop完要加上frequent
        '''
        
        def backtrack(comb, counter):
            
            if len(comb) == len(nums):
                results.append(list(comb))
                return

            for num in counter:
                if counter[num] > 0:
                    # add this number into the current combination
                    comb.append(num)
                    counter[num] -= 1
                    # continue the exploration
                    backtrack(comb, counter)
                    # revert the choice for the next exploration
                    comb.pop()
                    counter[num] += 1
                    
        results = []
        backtrack([], Counter(nums))

        return results
```



## [332. Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)

You are given a list of airline `tickets` where `tickets[i] = [fromi, toi]` represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

- For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg)

```
Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg)

```
Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.
```

 

**Constraints:**

- `1 <= tickets.length <= 300`
- `tickets[i].length == 2`
- `fromi.length == 3`
- `toi.length == 3`
- `fromi` and `toi` consist of uppercase English letters.
- `fromi != toi`



```python
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        '''
        出发的点是JFK，这里需要把所有ticket都连起来，最后的终点不能是JFK
        
        优先选择的是 按字母排序的
        '''
        
        '''
        这里的input是一个list，可以先去build一个map，然后再去调用map里面的数据
        iterative写法
        '''
        # get depts -> dests (with dests in reverse sorted order, since we're popping)
        dept_to_dests = defaultdict(list)
        #这里map需要先从大到小的顺序，这样存在dictionary里面的value顺序就是从大到小的。
        #value里面是从大到小的话，pop出来的顺序就是从小到大。
        for dept, dest in sorted(tickets, reverse=True):
            dept_to_dests[dept].append(dest)
        
        #这里build一个res route
        route = []
        #这里有一个stack，stack最开始的是jfk
        #这里就是从JFK往后面找
        stack = ['JFK']

        #只要是stack里面有票
        while stack:
            
            #只要是map里面还有stack对应的value，也就是意味着map里面还有tickets
            while dept_to_dests[stack[-1]]:
                #这里就是用map对应到的value, pop出来，存到stack里面，需要拿这个再去对应下一张ticket
                last = stack[-1]
                t_dest = dept_to_dests[last].pop()
                #把这个value 放到stack里面
                stack.append(t_dest)

            # 这里每一个ticket都会有用到
            # 所以如果有des在map里面没有找到对应的value
            # 就说明这个是最后要到达的点。
            # 这里就先append一个，然后接下来再append其他的路线
            route.append(stack.pop())
            
        return reversed(route)
    
    
#################################################################################

        targets = collections.defaultdict(list)
        for a, b in sorted(tickets)[::-1]:
            targets[a] += b,
        route = []
        def visit(airport):
            while targets[airport]:
                visit(targets[airport].pop())
            route.append(airport)
        visit('JFK')
        return route[::-1]
```





## [51. N-Queens](https://leetcode.com/problems/n-queens/)

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *all distinct solutions to the **n-queens puzzle***. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
```

**Example 2:**

```
Input: n = 1
Output: [["Q"]]
```

 

**Constraints:**

- `1 <= n <= 9`



```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        '''
        
        n代表有n个queen，和n*n的board
        避免queen的进攻是当前row, column, diagnal都不能有别的棋子
        
        
        这里的思路只有backtrack，因为只有一个个去试这里queen能放的位置有哪些
        那么除了backtrack，还需要一个valid function去判断是不是一个valid的位置
        O(N!N)
        '''
        
#         #这里is_valid需要去看当前的位置能不能放
#         def is_valid(row, col):
            
#             #把之前放queen的pos拿出来一个个对比
#             for pos in queen_pos:
#                 prev_row, prev_col = pos[0], pos[1]
                
#                 # 检查是不是diagnal
#                 # 公式： abs(y1-y2) == abs(x1-x2)
#                 if abs(prev_row - row) == abs(prev_col - col):
#                     return False
                
#                 # 检查是不是在同一个col
#                 if col == prev_col:
#                     return False
                
#                 #这里不用检查有没有row是因为一行就放了一个
#                 #所以其余情况都是True
#             return True

#         #这里backtrack里面要pass的variable有row，comb还有queen
#         #row用来check放的位置，comb里面就存一个queen，是相当于col的位置
#         #queen是用来check是不是所有的queen都放进去了
#         def backtrack(row, comb, queen):
#             #print(n, comb, queen)
            
#             # 如果所有的queen都放下了，可以放进去
#             if queen == 0:
#                 res.append(comb[:])
#                 return
            
#             # 如果row大过n，就不能加，超range了，直接return
#             if row >= n:
#                 return

#             #这里开始就是col的位置
#             for i in range(n):
#                 #新建一个position
#                 position = (row, i)
#                 # 如果valid，可以存当前这行到comb里
                
#                 #如果当前的这个位置是valid
#                 if is_valid(row, i):
                    
#                     #那么temp里面就存下当前的组合
#                     tmp = "." * i + "Q" + "." * (n - i - 1)
#                     #queen_pos里面存下，方便后面去check下面的position可以不可以放
#                     queen_pos.append(position)
#                     #comb里面存下现在的行
#                     comb.append(tmp)
#                     #再去查下一个level
#                     backtrack(row + 1, comb, queen - 1)
#                     #如果不可以，就要把现在存的comb和queen的position全部pop掉
#                     comb.pop()
#                     queen_pos.pop()
        
#         res = []
#         queen_pos = []
#         backtrack(0, [], n)
#         return res
    
        '''
        这里的思路就是创建一整个的board
        '''
        res = []
        board = [['.'] * n for _ in range(n)]
        
        #这里的长度肯定是再range里面的
        ldia = [False] * (2*n+1)
        rdia = [False] * (2*n+1)
        
        column = [False] * n
        
        def backtrack(row):
            
            if row == n:
                copy = ["".join(row) for row in board]
                res.append(copy)
                return

            for i in range(n):
                
                #对于斜角来说，斜率是1，(x1,y1)和(x2,y2).
                #(x2-x1)/(y2-y1) = 1 --> y2-y1 = x2-x1 --> y2 + x1 = x2 + y1 --> x1 - y1 = x2 - y2
                #按照斜率来说，左边的是正的，右边的是负斜率。不管是左还是右，总有一个是正还有一个负，这里就跟abs(x1-y1) = abs(x2-y2)是一个道理
                
                if column[i] or ldia[row - i] or rdia[row + i]:
                    continue
                
                board[row][i] = 'Q'
                
                column[i] = ldia[row - i] = rdia[row + i] = True

                backtrack(row + 1)
                #如果不行的话，就还原board的位置和True的位置
                board[row][i] = '.'
                column[i] = ldia[row - i] = rdia[row + i] = False


        backtrack(0)
        return res
```



## [37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

The `'.'` character indicates empty cells.

 

**Example 1:**

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

```
Input: board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
Output: [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
Explanation: The input board is shown above and the only valid solution is shown below:
```

 

**Constraints:**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` is a digit or `'.'`.
- It is **guaranteed** that the input board has only one solution.

```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        def is_valid(row, col, num):
            for i in range(9):
                
                if board[row][i] == num:
                    return False
                
                if board[i][col] == num:
                    return False
                
                if board[3*(row//3)+ i//3][3*(col//3)+ i%3] == num:
                    return False
                    
            return True


        def backtrack(row, col):

            if row ==9:
                return True
            
            if col == 9:
                return backtrack(row+1, 0)
            
            if board[row][col] == ".":
                for num in range(1,10):

                    if is_valid(row,col, str(num)):
                        board[row][col] = str(num)
                        if backtrack(row, col+1):
                            return True
                        else:
                            board[row][col] = "."
                return False
            else:
                return backtrack(row, col+1)

        backtrack(0, 0)
```

