# Interval

## Problems
1. [435. Non-overlapping Intervals](#435-non-overlapping-intervals)
2. [56. Merge Intervals](#56-merge-intervals)

# 435. Non-overlapping Intervals

### Problem Statement
Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return *the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping*.

**Note** that intervals which only touch at a point are **non-overlapping**. For example, `[1, 2]` and `[2, 3]` are non-overlapping.

### Examples

#### Example 1:
```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```

#### Example 2:
```
Input: intervals = [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.
```

#### Example 3:
```
Input: intervals = [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

#### Constraints:
* `1 <= intervals.length <= 10^5`
* `intervals[i].length == 2`
* `-5 * 10^4 <= starti < endi <= 5 * 10^4`

### 📌 Multiple Choice Questions: Greedy Approach  

#### 1️⃣ What is the key insight for solving this problem efficiently?  
- [ ] A) Sort intervals by their start times
- [ ] B) Sort intervals by their end times
- [ ] C) Sort intervals by their lengths (end - start)
- [ ] D) Use dynamic programming with memoization

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) Sort intervals by their end times**  
  
</details>  

---

#### 2️⃣ Why does sorting by end times work better than sorting by start times?  
- [ ] A) It's more efficient computationally
- [ ] B) It ensures we process intervals in chronological order
- [ ] C) It allows us to greedily select intervals that finish earliest, maximizing the number we can keep
- [ ] D) It simplifies the implementation of the algorithm

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) It allows us to greedily select intervals that finish earliest, maximizing the number we can keep**  
</details>  

---

#### 3️⃣ What is the relationship between "minimum number of intervals to remove" and "maximum number of non-overlapping intervals to keep"?  
- [ ] A) Minimum to remove = Total intervals - Maximum to keep
- [ ] B) Minimum to remove = Maximum to keep - Total intervals
- [ ] C) They are the same value
- [ ] D) There is no direct mathematical relationship

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) Minimum to remove = Total intervals - Maximum to keep**  
</details>  

---

#### 4️⃣ How do we determine if two intervals overlap in this problem?  
- [ ] A) If their start times are the same
- [ ] B) If their end times are the same
- [ ] C) If the start time of one interval is less than the end time of the other
- [ ] D) If the length of one interval is greater than the other

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) If the start time of one interval is less than the end time of the other**  
</details>  

---

#### 5️⃣ What is the time complexity of the optimal solution for this problem?  
- [ ] A) O(n), where n is the number of intervals
- [ ] B) O(n log n), where n is the number of intervals
- [ ] C) O(n²), where n is the number of intervals
- [ ] D) O(2ⁿ), where n is the number of intervals

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) O(n log n), where n is the number of intervals**  
</details>  

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if not intervals:
            return 0
            
        # Sort intervals by end time
        intervals.sort(key=lambda x: x[1])
        
        # Initialize with the first interval's end time
        current_end = intervals[0][1]
        # Count of non-overlapping intervals (we keep the first one)
        count = 1
        
        # Process the remaining intervals
        for i in range(1, len(intervals)):
            # If current interval starts at or after the previous end time
            if intervals[i][0] >= current_end:
                # We can include this interval
                count += 1
                # Update the current end time
                current_end = intervals[i][1]
        
        # Return the number of intervals to remove
        return len(intervals) - count
```

# 56. Merge Intervals

### Problem Statement
Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

### Examples

#### Example 1:
```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

#### Example 2:
```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

#### Constraints:
* `1 <= intervals.length <= 10^4`
* `intervals[i].length == 2`
* `0 <= starti <= endi <= 10^4`

### 📌 Multiple Choice Questions: Interval Merging Approach  

#### 1️⃣ Why do we sort the intervals array at the beginning of the solution?  
- [ ] A) To reduce the time complexity from O(n²) to O(n log n)  
- [ ] B) To ensure overlapping intervals are adjacent in the array  
- [ ] C) To make it easier to identify which intervals need to be merged  
- [ ] D) All of the above  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) All of the above**  
  
</details>  

---

#### 2️⃣ What is the significance of checking `start > res[-1][-1]` in the solution?  
- [ ] A) To verify that the current interval doesn't overlap with the last merged interval  
- [ ] B) To determine if a new non-overlapping interval should be added  
- [ ] C) To ensure the intervals are processed in the correct order  
- [ ] D) Both A and B  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) Both A and B**  
</details>  

---

#### 3️⃣ When we update `res[-1][-1] = max(res[-1][-1], end)`, what are we doing?  
- [ ] A) Finding the maximum end value among all intervals  
- [ ] B) Extending the end of the last merged interval if the current interval overlaps and extends further  
- [ ] C) Comparing all intervals to find the one with the largest end value  
- [ ] D) Creating a new interval to add to the result  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) Extending the end of the last merged interval if the current interval overlaps and extends further**  
</details>  

---

#### 4️⃣ Why do we initialize `res` with the first interval (`res = [intervals[0]]`)?  
- [ ] A) To have a starting point for interval comparisons  
- [ ] B) To handle the edge case of having only one interval  
- [ ] C) To avoid checking if `res` is empty in each iteration  
- [ ] D) All of the above  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) All of the above**  
</details>  

---

#### 5️⃣ What is the time complexity of this solution?  
- [ ] A) O(n), where n is the number of intervals  
- [ ] B) O(n log n), where n is the number of intervals  
- [ ] C) O(n²), where n is the number of intervals  
- [ ] D) O(n log n + n), which simplifies to O(n log n)  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) O(n log n + n), which simplifies to O(n log n)**  
</details>  

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort()

        res = [intervals[0]]

        for start, end in intervals:
            # print(start, res[-1][-1])
            if res == [] or start > res[-1][-1]:
                res.append([start, end])
            
            # elif res[-1][-1] > start:
            else:
                res[-1][-1] = max(res[-1][-1], end)
        
        return res
```
