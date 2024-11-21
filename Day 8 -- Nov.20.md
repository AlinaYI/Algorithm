# [344. Reverse String](https://leetcode.com/problems/reverse-string/)

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with `O(1)` extra memory.

 

**Example 1:**

```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

**Example 2:**

```
Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

 

**Constraints:**

- `1 <= s.length <= 105`
- `s[i]` is a [printable ascii character](https://en.wikipedia.org/wiki/ASCII#Printable_characters).



```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        #two pointer
        # On
        # O1
        
        left,right = 0,len(s)-1
        
        while left < right:
            
            temp = s[left]
            s[left] = s[right]
            s[right] = temp
            
            left += 1
            right -= 1
        
        return s
    
```



# [541. Reverse String II](https://leetcode.com/problems/reverse-string-ii/)

Given a string `s` and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.

If there are fewer than `k` characters left, reverse all of them. If there are less than `2k` but greater than or equal to `k` characters, then reverse the first `k` characters and leave the other as original.

 

**Example 1:**

```
Input: s = "abcdefg", k = 2
Output: "bacdfeg"
```

**Example 2:**

```
Input: s = "abcd", k = 2
Output: "bacd"
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of only lowercase English letters.
- `1 <= k <= 104`



```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        # On On
        a = list(s)
        for i in xrange(0, len(a), 2*k):
            a[i:i+k] = reversed(a[i:i+k])
        return "".join(a)

########################################################################
        '''
        tc: O(n)
        sc : O(1)

         把每一组单拎出来reverse
        '''
        def reverse(s):

            left, right = 0, len(s)-1

            while left < right:

                temp = s[left]
                s[left] = s[right]
                s[right] = temp

                left += 1
                right -= 1
            return s

        # 变成list
        s = list(s)
        # 这里initial的是第一组
        left = 0
        right = k

        # 这里找出left的limit
        while left < len(s):
            # 把第一组先reverse
            print(s[left:right])
            s[left:right] = reverse(s[left:right])

            # 然后update 两个pointer 的位置
            left = 2*k + left
            right = len(s) if right > len(s) else left + k

        return "".join(s)
```

