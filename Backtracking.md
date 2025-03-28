# Backtracking

## Problems
1. [1415. The k-th Lexicographical String of All Happy Strings of Length n](#1415-the-k-th-lexicographical-string-of-all-happy-strings-of-length-n)
2. [2375. Construct Smallest Number From DI String](#2375-construct-smallest-number-from-di-string)
3. 

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

# 2375. Construct Smallest Number From DI String

### Problem Statement
You are given a 0-indexed string `pattern` of length `n` consisting of the characters 'I' meaning increasing and 'D' meaning decreasing.

A 0-indexed string `num` of length `n + 1` is created using the following conditions:
- `num` consists of the digits '1' to '9', where each digit is used at most once.
- If `pattern[i] == 'I'`, then `num[i] < num[i + 1]`.
- If `pattern[i] == 'D'`, then `num[i] > num[i + 1]`.

Return the lexicographically smallest possible string `num` that meets the conditions.

### Examples

#### Example 1:
```
Input: pattern = "IIDD"
Output: "12543"
Explanation:
The digits 1, 2, 5, 4, and 3 are used to form the string.
"I" - 1 < 2
"I" - 2 < 5
"D" - 5 > 4
"D" - 4 > 3
Since "12543" is the lexicographically smallest string, we return it.
```

#### Example 2:
```
Input: pattern = "DDD"
Output: "4321"
Explanation:
The digits 4, 3, 2, and 1 are used to form the string.
"D" - 4 > 3
"D" - 3 > 2
"D" - 2 > 1
Since "4321" is the lexicographically smallest string, we return it.
```

#### Constraints:
* `1 <= pattern.length <= 8`
* `pattern` consists of only the letters 'I' and 'D'.

### 📌 Multiple Choice Questions: Backtracking Approach  

#### 1️⃣ What is the key constraint that makes this problem challenging?  
- [ ] A) Finding a string that's exactly n+1 characters long
- [ ] B) Using only the digits 1-9 in the output string
- [ ] C) Using each digit at most once while satisfying the I/D pattern
- [ ] D) Creating a string that's in ascending order

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) Using each digit at most once while satisfying the I/D pattern**  
  
  The constraint of using each digit at most once (no repetition) while still satisfying the increasing/decreasing pattern makes this a backtracking problem rather than a simpler greedy approach.
</details>  

---

#### 2️⃣ How does the solution determine the range of possible values for each position?  
- [ ] A) It always tries digits 1 through 9 in every position
- [ ] B) It uses different ranges based on whether the pattern is 'I' (increasing) or 'D' (decreasing)
- [ ] C) It uses a predefined set of ranges based on the position in the string
- [ ] D) It picks random digits and verifies if they follow the pattern

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) It uses different ranges based on whether the pattern is 'I' (increasing) or 'D' (decreasing)**  
  
  For an 'I' pattern at position idx, the solution tries values greater than the previous digit (smallest_string[idx] + 1 to 9).
  For a 'D' pattern, it tries values less than the previous digit (1 to smallest_string[idx] - 1).
</details>  

---

#### 3️⃣ What data structure is used to ensure each digit is used at most once?  
- [ ] A) An array of boolean flags
- [ ] B) A set to track seen digits
- [ ] C) A counter for each digit
- [ ] D) A priority queue

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) A set to track seen digits**  
  
  The solution uses a set (`seen`) to keep track of which digits have already been used, ensuring no digit is used more than once.
</details>  

---

#### 4️⃣ What optimization technique is used in the backtracking approach?  
- [ ] A) Memoization to avoid redundant calculations
- [ ] B) Branch and bound to prune unpromising paths
- [ ] C) Early termination once a valid solution is found
- [ ] D) Sorting potential digits to try smaller ones first

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) Early termination once a valid solution is found**  
  
  The backtracking returns True as soon as it finds a valid solution, which propagates up the recursion stack to terminate the search. Since digits are tried in ascending order, the first valid solution will be the lexicographically smallest.
</details>  

---

#### 5️⃣ What is the time complexity of this backtracking solution in the worst case?  
- [ ] A) O(n), where n is the length of the pattern
- [ ] B) O(9^n), where n is the length of the pattern
- [ ] C) O(9!), where 9 is the maximum number of digits we can use
- [ ] D) O(2^n), where n is the length of the pattern

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) O(9!), where 9 is the maximum number of digits we can use**  
  
  In the worst case, we might need to try all permutations of digits 1-9, which is 9! arrangements. However, the actual complexity is often much lower due to the pattern constraints and early termination.
</details>  

```python
class Solution:
    def smallestNumber(self, pattern: str) -> str:
        '''
        Approach: Backtracking
        
        1. Use backtracking to build the smallest string that satisfies the pattern
        2. For each position, try digits from smallest to largest that satisfy the pattern
        3. Use a set to track used digits to ensure no repetition
        4. Return early once a valid solution is found
        
        The pattern determines the relationship between adjacent positions:
        - 'I': next digit must be larger than current
        - 'D': next digit must be smaller than current
        '''
        def backtracking(idx: int) -> bool:
            nonlocal smallest_string, seen
            
            # Base case: if we've processed the entire pattern, we've found a solution
            if idx == len(pattern):
                return True
            
            # First position is special (-1), or determine range based on pattern
            if idx == -1:
                start = 1  # For first position, start with smallest digit
                end = 10   # Try all digits 1-9
            elif pattern[idx] == "I":
                # Increasing pattern: next digit must be larger than current
                start = smallest_string[idx] + 1
                end = 10
            else:  # pattern[idx] == "D"
                # Decreasing pattern: next digit must be smaller than current
                start = 1
                end = smallest_string[idx]
            
            # Try each possible digit in the valid range
            for i in range(start, end):
                if i not in seen:
                    seen.add(i)  # Mark digit as used
                    smallest_string[idx + 1] = i  # Place digit in the string
                    
                    # Recursively try to place the next digit
                    if backtracking(idx + 1):
                        return True  # Found a valid solution
                    
                    # Backtrack: remove the digit for other possibilities
                    seen.remove(i)
            
            return False  # No valid solution found with current configuration
        
        # Initialize string with one more position than pattern length
        smallest_string = [0] * (len(pattern) + 1)
        seen = set()  # To track used digits
        
        # Start backtracking from before the first position (special case)
        backtracking(-1)
        
        # Convert integer array to string
        return "".join(map(str, smallest_string))
```
