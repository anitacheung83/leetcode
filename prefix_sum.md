# Leetcode Prefix Sum

## 1749. Maximum Absolute Sum of Any Subarray

### Problem Statement

You are given an integer array `nums`. The **absolute sum** of a subarray `[numsl, numsl+1, ..., numsr-1, numsr]` is `abs(numsl + numsl+1 + ... + numsr-1 + numsr)`.

Return the **maximum** absolute sum of any **(possibly empty)** subarray of `nums`.

Note that `abs(x)` is defined as follows:
* If `x` is a negative integer, then `abs(x) = -x`.
* If `x` is a non-negative integer, then `abs(x) = x`.

### Examples

#### Example 1:

```
Input: nums = [1,-3,2,3,-4]
Output: 5
Explanation: The subarray [2,3] has absolute sum = abs(2+3) = abs(5) = 5.
```

#### Example 2:

```
Input: nums = [2,-5,1,-4,3,-2]
Output: 8
Explanation: The subarray [-5,1,-4] has absolute sum = abs(-5+1-4) = abs(-8) = 8.
```

#### Constraints:
* `1 <= nums.length <= 10^5`
* `-10^4 <= nums[i] <= 10^4`

### 📌 Multiple Choice Questions: Greedy - Prefix Sum Approach  

#### 1️⃣ What is the main idea behind the Prefix Sum approach for finding the maximum absolute subarray sum?  
- [ ] A) Finding the largest contiguous positive subarray  
- [ ] B) Calculating the difference between the largest and smallest prefix sums  
- [ ] C) Sorting the array and selecting the largest absolute difference  
- [ ] D) Using a sliding window to track the highest absolute difference  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) Calculating the difference between the largest and smallest prefix sums**  
  
</details>  

---

#### 2️⃣ Why do we initialize `minPrefixSum` and `maxPrefixSum` to 0 instead of extreme values like INT_MIN or INT_MAX?  
- [ ] A) To handle cases where all elements are negative  
- [ ] B) To ensure we correctly compute prefix sums from the start  
- [ ] C) To allow an empty subarray (sum = 0) as a valid choice  
- [ ] D) All of the above  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) All of the above**  
</details>  

---

#### 3️⃣ What is the purpose of tracking both `minPrefixSum` and `maxPrefixSum` during the iteration?  
- [ ] A) To compute the sum of all subarrays efficiently  
- [ ] B) To keep track of the largest and smallest values encountered for maximizing the absolute sum  
- [ ] C) To determine the longest increasing subsequence  
- [ ] D) To optimize the time complexity to \(O(\log N)\)  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) To keep track of the largest and smallest values encountered for maximizing the absolute sum**  
</details>  

---

#### 4️⃣ What is the time complexity of the Prefix Sum approach to finding the maximum absolute subarray sum?  
- [ ] A) \(O(N^2)\)  
- [ ] B) \(O(\log N)\)  
- [ ] C) \(O(N)\)  
- [ ] D) \(O(1)\)  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) \(O(N)\)**  
</details>  

---

#### 5️⃣ Which of the following statements is **false** about the Prefix Sum approach?  
- [ ] A) It requires additional space proportional to the input array size  
- [ ] B) It updates `prefixSum` at each iteration  
- [ ] C) It ensures that `maxPrefixSum - minPrefixSum` gives the maximum absolute subarray sum  
- [ ] D) It uses a single pass through the array to compute the result  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) It requires additional space proportional to the input array size**  
</details>  

```python
class Solution:
    def maxAbsoluteSum(self, nums: List[int]) -> int:
        min_prefix_sum = 0
        max_prefix_sum = 0

        prefix_sum = 0

        for num in nums:
            prefix_sum += num

            min_prefix_sum = min(min_prefix_sum, prefix_sum)
            max_prefix_sum = max(max_prefix_sum, prefix_sum)

        return max_prefix_sum - min_prefix_sum
```
