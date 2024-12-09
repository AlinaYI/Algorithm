# Recursion

回溯，就是自己call 自己

```markdown
def func(): <--
              |
              | (recursive call)
              |
    func() ----
```

回溯是递归的副产品，只要有递归就会有回溯。



### 回溯法的效率

**因为回溯的本质是穷举，穷举所有可能，然后选出我们想要的答案**，如果想让回溯法高效一些，可以加一些剪枝的操作，但也改不了回溯法就是穷举的本质。



### 回溯法解决的问题

回溯法，一般可以解决如下几种问题：

- 组合问题：N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后，解数独等等





## [77. Combinations](https://leetcode.com/problems/combinations/)

Given two integers `n` and `k`, return *all possible combinations of* `k` *numbers chosen from the range* `[1, n]`.

You may return the answer in **any order**.

 

**Example 1:**

```
Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Explanation: There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.
```

**Example 2:**

```
Input: n = 1, k = 1
Output: [[1]]
Explanation: There is 1 choose 1 = 1 total combination.
```

 

**Constraints:**

- `1 <= n <= 20`
- `1 <= k <= n`



```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        
        '''
        这道题是要找到1，n的所有k长度的排列组合
        因为是要存到list里面，generate所有的combination所以只能是brute force，
            那么brunte force就是：recursion，dfs，backtracking
        
        backtracking就是decision tree
        也就是选了1个数字之后，还能选择其他几个数字， hight of tree就是2
              *
        /    /   \  \
       1    2    3  4
      //\   /\   |
      23 4  3 4  4

      第一层是选n，第二层是选n-1
      n^k  k就是树高
        '''

        # 这个用来存所有的数字
        res = []

        # 选择pass start是因为要看从哪里开始的
        # comb就是看现在存了哪些数字在里面
        def backtrack(start, comb):
            # base，找到k个了
            if len(comb) == k:
                # 因为这里后面还需要修改，找其他的comb
                # 如果直接加comb在res里面，后面可能会修改道
                # 所以这里加的是copy
                res.append(comb.copy())
                return

            '''
            这里剪枝就是，让for loop的范围缩小
            '''
            need = k - len(comb)
            remain = n - start + 1
            available = remain - need

            # 这里开始make decision
            for i in range(start, start + available + 1):
                comb.append(i)
                backtrack(i + 1, comb)
                # remove the number
                comb.pop()

        backtrack(1, [])
        return res
```





## [216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)

Find all valid combinations of `k` numbers that sum up to `n` such that the following conditions are true:

- Only numbers `1` through `9` are used.
- Each number is used **at most once**.

Return *a list of all possible valid combinations*. The list must not contain the same combination twice, and the combinations may be returned in any order.

 

**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]
Explanation:
1 + 2 + 4 = 7
There are no other valid combinations.
```

**Example 2:**

```
Input: k = 3, n = 9
Output: [[1,2,6],[1,3,5],[2,3,4]]
Explanation:
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
There are no other valid combinations.
```

**Example 3:**

```
Input: k = 4, n = 1
Output: []
Explanation: There are no valid combinations.
Using 4 different numbers in the range [1,9], the smallest sum we can get is 1+2+3+4 = 10 and since 10 > 1, there are no valid combination.
```

 

**Constraints:**

- `2 <= k <= 9`
- `1 <= n <= 60`



```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        
        '''
        这道题跟combination是一样的
        那么就是backtrack过程中，算一下sum
        base case就是改变一下

        剪枝就是 如果sum大于n，直接break
        '''
        # 这里存所有等于n的组合
        res = []

        # 这里back track还是start和comb
        def backtrack(start, comb):
            if len(comb) == k and sum(comb) == n:
                res.append(comb.copy())
                return

            for i in range(start, 10):

                '''
                这里剪枝在for loop里面
                如果剩下的数字不够k个
                '''
                if k > (10-i) + len(comb):
                    break

                comb.append(i)
                backtrack(i+1, comb)
                comb.pop()

        backtrack(1, [])
        return res
```





## [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![img](https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png)

 

**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**Example 2:**

```
Input: digits = ""
Output: []
```

**Example 3:**

```
Input: digits = "2"
Output: ["a","b","c"]
```

 

**Constraints:**

- `0 <= digits.length <= 4`
- `digits[i]` is a digit in the range `['2', '9']`.



```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        '''
        这道题也是枚举，需要把所有的可能都列举出来
        backtrack
        这里的范围就是数字所对应的字母的组合

        这道题跟combination不一样的地方就是
        这里backtrack是string，string在python里面是immutable
        所以这里在append到res里面是不用加copy，
        递归时都会传递一个新的值给 comb，无需进行 pop 操作来恢复原状态
        '''

        # 这里先建立一个char
        # O 4^n n
        # On
        digit_char = {
            "2": ["a", "b", "c"],
            "3": ["d", "e", "f"],
            "4": ["g", "h", "i"],
            "5": ["j", "k", "l"],
            "6": ["m", "n", "o"],
            "7": ["p", "q", "r", "s"],
            "8": ["t", "u", "v"],
            "9": ["w", "x", "y", "z"] 
        }

        # 用来存所有的结果
        res = []

        def backtrack(start, comb):

            if len(comb) == len(digits):
                res.append(comb)
                return

            for char in digit_char[digits[start]]:
                backtrack(start+1, comb + char)

        res = []
        if digits:
            backtrack(0, "")
        return res
```

