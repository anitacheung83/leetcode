# 1D Dynamic Programming Tabulation

# 198. House Robber

### Problem Statement
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

### Examples

#### Example 1:
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

#### Example 2:
```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

#### Constraints:
* `1 <= nums.length <= 100`
* `0 <= nums[i] <= 400`

### 📌 Multiple Choice Questions: Dynamic Programming Approach  

#### 1️⃣ What is the key constraint in the House Robber problem?  
- [ ] A) You can only rob houses with even-indexed positions
- [ ] B) You cannot rob adjacent houses
- [ ] C) You must rob at least one house
- [ ] D) You can rob at most half of the houses

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) You cannot rob adjacent houses**  
  
  The key constraint is that you cannot rob adjacent houses because the security systems will alert the police if two adjacent houses are robbed on the same night.
</details>  

---

#### 2️⃣ What does the array `max_money[i]` represent in the solution?  
- [ ] A) The amount of money in house i
- [ ] B) Whether to rob house i (1) or not (0)
- [ ] C) The maximum amount of money that can be robbed up to house i
- [ ] D) The amount of money gained by robbing house i

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) The maximum amount of money that can be robbed up to house i**  
  
  The array `max_money[i]` stores the maximum amount of money that can be robbed when considering houses from index 0 to index i.
</details>  

---

#### 3️⃣ What is the recurrence relation used in this dynamic programming solution?  
- [ ] A) max_money[i] = max_money[i-1] + nums[i]
- [ ] B) max_money[i] = max(max_money[i-1], nums[i])
- [ ] C) max_money[i] = max(max_money[i-1], max_money[i-2] + nums[i])
- [ ] D) max_money[i] = max_money[i-1] + max_money[i-2]

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) max_money[i] = max(max_money[i-1], max_money[i-2] + nums[i])**  
  
  At each house, you have two options:
  1. Skip this house (take max_money[i-1])
  2. Rob this house + maximum money you could get from houses before the previous one (max_money[i-2] + nums[i])
  The maximum of these two options is the optimal solution for house i.
</details>  

---

#### 4️⃣ What are the base cases in this dynamic programming solution?  
- [ ] A) max_money[0] = 0 and max_money[1] = nums[0]
- [ ] B) max_money[0] = nums[0] and max_money[1] = max(nums[0], nums[1])
- [ ] C) max_money[0] = nums[0] and max_money[1] = nums[1]
- [ ] D) max_money[0] = nums[0] and max_money[1] = nums[0] + nums[1]

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) max_money[0] = nums[0] and max_money[1] = max(nums[0], nums[1])**  
  
  The base cases are:
  - For the first house (index 0), the maximum money you can rob is simply the amount in that house.
  - For the second house (index 1), you can either rob this house or the first house, so the maximum is the larger of the two.
</details>  

---

#### 5️⃣ What is the time and space complexity of this solution?  
- [ ] A) Time: O(n), Space: O(n)
- [ ] B) Time: O(n²), Space: O(n)
- [ ] C) Time: O(n), Space: O(1)
- [ ] D) Time: O(2ⁿ), Space: O(n)

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) Time: O(n), Space: O(n)**  
  
  - Time complexity is O(n) because we process each house exactly once, performing constant time operations.
  - Space complexity is O(n) for the `max_money` array that stores results for each house.
  
  Note: This solution could be optimized to use O(1) space by only keeping track of the previous two results instead of the entire array.
</details>  

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        '''
        Approach: Dynamic Programming (Bottom-Up)
        
        1. Define a DP array where max_money[i] represents the maximum amount
           that can be robbed up to house i
        2. For each house, we have two choices:
           - Skip this house (take max_money[i-1])
           - Rob this house + max money from houses before the previous one (max_money[i-2] + nums[i])
        3. Take the maximum of these two choices
        
        Time Complexity: O(n)
        Space Complexity: O(n)
        '''
        # Handle the edge case where there's only one house
        if len(nums) < 2:
            return nums[0]
        
        # Initialize the DP array
        max_money = [0] * len(nums)
        
        # Base cases
        max_money[0] = nums[0]  # If there's only one house, rob it
        max_money[1] = max(nums[0], nums[1])  # For two houses, rob the one with more money
        
        # Fill the DP array using the recurrence relation
        for i in range(2, len(nums)):
            # Either skip the current house or rob it plus the max money from houses before i-1
            max_money[i] = max(max_money[i - 1], max_money[i - 2] + nums[i])
        
        # The last element contains the maximum possible amount
        return max_money[-1]
```



