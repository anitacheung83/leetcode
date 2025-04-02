# Array

1. [151. Reverse Words in a String](#-151-reverse-words-in-a-string)

# 151. Reverse Words in a String

## Problem Statement
Given an input string `s`, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in `s` will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

## Examples

Example 1:
```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

Example 2:
```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

Example 3:
```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

## Constraints:
* `1 <= s.length <= 10^4`
* `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
* There is at least one word in `s`.

## 📌 Multiple Choice Questions: String Manipulation Approach

### 1️⃣ What is the main challenge in reversing words in a string?
* A) Handling multiple consecutive spaces
* B) Determining where each word begins and ends
* C) Managing leading and trailing spaces
* D) All of the above

### 💡 Answer
D) All of the above

### 2️⃣ Why does the solution use word accumulation rather than string splitting?
* A) To minimize memory usage
* B) To handle spaces more precisely
* C) To achieve better time complexity
* D) Both A and B

### 💡 Answer
D) Both A and B

### 3️⃣ What does `words[::-1]` do in the solution?
* A) Reverses each individual word
* B) Reverses the order of words in the list
* C) Removes spaces from each word
* D) Filters out empty words

### 💡 Answer
B) Reverses the order of words in the list

### 4️⃣ What is the purpose of checking `if word != ""` before appending to the words list?
* A) To avoid adding empty strings that result from consecutive spaces
* B) To handle leading spaces
* C) To handle trailing spaces
* D) All of the above

### 💡 Answer
A) To avoid adding empty strings that result from consecutive spaces

### 5️⃣ What is the time complexity of this solution?
* A) O(n), where n is the length of the string
* B) O(n²), where n is the length of the string
* C) O(w), where w is the number of words
* D) O(n log n), where n is the length of the string

### 💡 Answer
A) O(n), where n is the length of the string

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        words = []
        word = ''
        
        for i in range(len(s)):
            if s[i] != ' ':
                word = word + s[i]
            else:
                if word != "":
                    words.append(word)
                    word = ""
        
        if word != "":
            words.append(word)
        
        return " ".join(words[::-1])
```
