# 2D Dynamic Programming Tabulation
## Problems
1. [1143 Longest Common Subsequence](#1143-longest-common-subsequence)
2. [97. Interleaving String](#97-interleaving-string)

   
# 1143 Longest Common Subsequence

### Problem Statement
Given two strings `text1` and `text2`, return the length of their longest common subsequence. If there is no common subsequence, return `0`.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, `"ace"` is a subsequence of `"abcde"`.
A **common subsequence** of two strings is a subsequence that is common to both strings.

 
### Examples
#### Example 1:
```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```
#### Example 2:
```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

#### Example 3:
```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

#### Constraints:

* `1 <= text1.length, text2.length <= 1000`
* `text1 and text2 consist of only lowercase English characters.`

### Steps
1. Top Down Approach
2. Convert Top Down to Bottom Up

# 72. Edit Distance

# 97. Interleaving String

### Problem Statement
Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an interleaving of `s1` and `s2`.

An interleaving of two strings s and t is a configuration where s and t are divided into n and m substrings respectively, such that:

* `s = s1 + s2 + ... + sn`
* `t = t1 + t2 + ... + tm`
* `|n - m| <= 1`
The interleaving is `s1 + t1 + s2 + t2 + s3 + t3 + ... or t1 + s1 + t2 + s2 + t3 + s3 + ...`
Note: `a + b` is the concatenation of strings `a` and `b`.

 
### Examples
#### Example 1:

![image](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
Explanation: One way to obtain s3 is:
Split s1 into s1 = "aa" + "bc" + "c", and s2 into s2 = "dbbc" + "a".
Interleaving the two splits, we get "aa" + "dbbc" + "bc" + "a" + "c" = "aadbbcbcac".
Since s3 can be obtained by interleaving s1 and s2, we return true.
```

#### Example 2:
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
Explanation: Notice how it is impossible to interleave s2 with any other string to obtain s3.
Example 3:

Input: s1 = "", s2 = "", s3 = ""
Output: true
```

### Constraints:

* `0 <= s1.length, s2.length <= 100`
* `0 <= s3.length <= 200`
* `s1, s2, and s3 consist of lowercase English letters.`

### Steps:

#### Recursion with memoization

```Python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        '''
        Approach: Recursion with memoirization
        Base Case:

        if len(string) == len(s3):
            return 

        '''

        def backtrack(i: int, j: int) -> bool:
            # Base Case
            k = i + j
            if k == o:
                return True
            
            if memo[i][j] != -1:
                return memo[i][j]

            # Inductive Step
            if i < n and s1[i] == s3[k] and backtrack(i + 1, j):
                memo[i + 1][j] = 1
                return True
            
            if j < m and s2[j] == s3[k] and backtrack(i, j + 1): 
                memo[i][j + 1] = 1
                return True

            memo[i][j] = 0
            return False
            

        
        n, m, o = len(s1), len(s2), len(s3)

        if n + m != o:
            return False
        
        memo = [[-1] * (m + 1) for _ in range(n + 1)]

        return backtrack(0, 0)
```

#### 2. Form the grid
* x: s1[i]
* y: s2[j]


Example 1
### DP Table for Interleaving Strings

|       | `""` | `d`  | `b`  | `b`  | `c`  | `a` |
|-------|----|----|----|----|----|----|
| `""`   | ✅  | ❌  | ❌  | ❌  | ❌  | ❌  |
| `a`    | ✅  | ❌  | ❌  | ❌  | ❌  | ❌  |
| `a`    | ✅  | ✅  | ✅  | ✅  | ❌  | ❌  |
| `b`   | ❌  | ✅  | ✅  | ✅  | ✅  | ❌  |
| `c`    | ❌  | ❌  | ✅  | ✅  | ✅  | ✅  |
| `c`    | ❌  | ❌  | ❌  | ✅  | ✅  | ✅  |

Legend:  
✅ = `True` (interleaving possible up to that point)  
❌ = `False` (interleaving not possible)

```Python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        '''
        Approach: DP memoization
        '''

        if len(s1) + len(s2) != len(s3):
            return False 

        dp = [[0] * (len(s2) + 1) for _ in range(len(s1) + 1)]
        dp[0][0] = 1


        for i in range(1, len(s1) + 1):
            dp[i][0] = dp[i - 1][0] and (s1[i - 1] == s3[i - 1])

        # Fill first row (using s2 only)
        for j in range(1, len(s2) + 1):
            dp[0][j] = dp[0][j - 1] and (s2[j - 1] == s3[j - 1])

        for i in range(1, len(s1) + 1):
            for j in range(1, len(s2) + 1):
                dp[i][j] = (dp[i][j - 1] and s3[i + j - 1] == s2[j - 1]) or (dp[i - 1][j] and s3[ i - 1 + j] == s1[i - 1])
                
               
        return dp[-1][-1]
```

        

        
