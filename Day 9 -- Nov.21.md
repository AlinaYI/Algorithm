# [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space.*

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

 

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**

```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

**Example 3:**

```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
- There is **at least one** word in `s`.



```python
class Solution(object):
    def reverseWords(self, s):
        #这道题需要的就是把整个string里面，每个单词分别reverse，然后整个string reverse
        #这里就是把string里面的单词根据空格来split，然后再按照reverse的顺序加入string
        # O(n) O(n)
        
        s = s.split(" ")  #split,and get a list
        res = ''          #initial res
        rever = s[::-1]   #reverse the list 先reverse list
        
        for i in rever:
            if str(i) != "":
                res += str(i) + ' '
        return res[:-1]

```



# 字符串总结

常见问题: 

​	双指针： 两个pointer 分别指向哪里，怎么移动

​	反转字符：当需要固定规律一段一段去处理字符串的时候，要想想在在for循环的表达式上做做文章

​			   **先整体反转再局部反转**，实现了反转字符串里的单词



