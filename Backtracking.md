# Backtracking

# 1415. The k-th Lexicographical String of All Happy Strings of Length n

### Problem Statement
A happy string is a string that:
- Consists only of letters of the set ['a', 'b', 'c'].
- No letter appears consecutively (i.e., s[i] != s[i + 1] for all 1 <= i < s.length).

You are given two integers `n` and `k`. Return the k-th lexicographically smallest happy string of length n. If there are fewer than k happy strings of length n, return an empty string.

Note that k is 1-indexed, meaning the first lexicographically smallest happy string has index 1, the second has index 2, and so on.

### Examples

#### Example 1:
```
Input: n = 1, k = 3
Output: "c"
Explanation: The happy strings of length 1 are "a", "b", and "c". The 3rd one is "c".
```

#### Example 2:
```
Input: n = 1, k = 4
Output: ""
Explanation: There are only 3 happy strings of length 1.
```

#### Example 3:
```
Input: n = 3, k = 9
Output: "cab"
Explanation: There are 12 happy strings of length 3: "aba", "abc", "aca", "acb", "bab", "bac", "bca", "bcb", "cab", "cac", "cba", "cbc". The 9th one is "cab".
```

#### Constraints:
* `1 <= n <= 10`
* `1 <= k <= 100`

### 📌 Multiple Choice Questions: Backtracking Approach  

#### 1️⃣ What makes a string "happy" according to the problem definition?  
- [ ] A) It must contain all the letters 'a', 'b', and 'c'
- [ ] B) It must have an equal number of each letter
- [ ] C) No letter appears consecutively (i.e., s[i] != s[i + 1])
- [ ] D) It must have an equal number of vowels and consonants

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) No letter appears consecutively (i.e., s[i] != s[i + 1])**  
  
  The primary constraint that makes a string "happy" is that no adjacent characters can be the same. The string also must only use the letters 'a', 'b', and 'c'.
</details>  

---

#### 2️⃣ What technique is used to find the k-th lexicographically smallest happy string?  
- [ ] A) Dynamic Programming
- [ ] B) Backtracking
- [ ] C) Union-Find
- [ ] D) Binary Search

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) Backtracking**  
  
  The solution uses backtracking to generate happy strings in lexicographical order, stopping once it reaches the k-th string.
</details>  

---

#### 3️⃣ How does the solution optimize for finding exactly the k-th string?  
- [ ] A) By generating all possible strings first, then sorting them
- [ ] B) By using a mathematical formula to compute the k-th string directly
- [ ] C) By stopping the backtracking process once the k-th string is found
- [ ] D) By using a queue to keep track of the order of strings

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) By stopping the backtracking process once the k-th string is found**  
  
  The solution optimizes by returning true once the k-th string is found, which propagates up the recursion stack and stops further exploration of the search space.
</details>  

---

#### 4️⃣ What is the purpose of the condition `if idx == 0 or string[idx - 1] != letter` in the backtracking function?  
- [ ] A) To ensure all letters are used at least once
- [ ] B) To enforce the "happy string" constraint that no letter appears consecutively
- [ ] C) To maintain lexicographical order
- [ ] D) To limit the string length to n

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) To enforce the "happy string" constraint that no letter appears consecutively**  
  
  This condition checks that the current letter being considered is different from the previous letter in the string, which is the core constraint of a happy string.
</details>  

---

#### 5️⃣ What is the time complexity of this backtracking solution?  
- [ ] A) O(n), where n is the length of the string
- [ ] B) O(3^n), where n is the length of the string
- [ ] C) O(min(k, 3 * 2^(n-1))), where n is the length of the string and k is the target index
- [ ] D) O(k * n), where n is the length of the string and k is the target index

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) O(min(k, 3 * 2^(n-1))), where n is the length of the string and k is the target index**  
  
  The time complexity is proportional to the number of strings we need to generate before finding the k-th one. At most, we need to generate k strings. There are 3 * 2^(n-1) possible happy strings of length n, so we generate at most min(k, 3 * 2^(n-1)) strings.
</details>  

```python
class Solution:
    def getHappyString(self, n: int, k: int) -> str:
        '''
        Approach: Backtracking
        
        1. Use backtracking to generate happy strings in lexicographical order
        2. Stop once we've found the k-th string
        3. Return the k-th string or "" if there are fewer than k happy strings
        
        A happy string:
        - Contains only letters 'a', 'b', 'c'
        - No letter appears consecutively (s[i] != s[i + 1])
        '''

        def backtrack(idx: int):
            nonlocal strings, string, letters
            
            if idx == n:
                # Once we've built a string of length n, add it to our result list
                strings.append(string.copy())
                # If we've found the k-th string, we can stop
                if len(strings) == k:
                    return True
                return
            
            for letter in letters:
                # We can use this letter if:
                # 1. It's the first character (idx == 0), or
                # 2. It's different from the previous character
                if idx == 0 or string[idx - 1] != letter:
                    string[idx] = letter
                    # If we've found the k-th string in a deeper recursion,
                    # propagate the success signal up
                    if backtrack(idx + 1):
                        return True
            
            return False

        strings = []
        string = [None] * n  # Preallocate to avoid memory reallocation
        letters = ['a', 'b', 'c']
        
        # Start backtracking from index 0
        backtrack(0)
        
        # Return the k-th string if found, otherwise an empty string
        return "".join(strings[-1]) if len(strings) == k else ""
```
