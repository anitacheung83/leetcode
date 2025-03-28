# LeetCode Binary Search

# Problems
1. [875. Koko Eating Bananas](#875-koko-eating-bananas)
2. [2300. Successful Pairs of Spells and Potions](#2300-successful-pairs-of-spells-and-potions)
3. [162. Find Peak Element](#162-find-peak-element)

# 875. Koko Eating Bananas

### Problem Statement
Koko loves to eat bananas. There are `n` piles of bananas, the `i`th pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses one pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

### Examples

#### Example 1:
```
Input: piles = [3,6,7,11], h = 8
Output: 4
```

#### Example 2:
```
Input: piles = [30,11,23,4,20], h = 5
Output: 30
```

#### Example 3:
```
Input: piles = [30,11,23,4,20], h = 6
Output: 23
```

#### Constraints:
* `1 <= piles.length <= 10^4`
* `piles.length <= h <= 10^9`
* `1 <= piles[i] <= 10^9`

### 📌 Multiple Choice Questions: Binary Search Approach  

#### 1️⃣ What is the main idea behind using Binary Search to find the minimum eating speed?  
- [ ] A) Finding a valid speed by trying every possible value linearly  
- [ ] B) Using divide and conquer to split the banana piles  
- [ ] C) Narrowing down the search space of possible speeds efficiently  
- [ ] D) Sorting the piles to optimize the banana consumption  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) Narrowing down the search space of possible speeds efficiently**  
  
</details>  

---

#### 2️⃣ Why do we initialize `left = 1` and `right = max(piles)` for the binary search?  
- [ ] A) To ensure we start with the minimum possible speed (1) and the maximum needed speed  
- [ ] B) Because binary search always requires the initial boundaries to be 1 and the maximum value  
- [ ] C) To avoid integer overflow in the calculations  
- [ ] D) Because Koko must eat at least one banana but never needs to eat more than the largest pile in one hour  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) Because Koko must eat at least one banana but never needs to eat more than the largest pile in one hour**  
</details>  

---

#### 3️⃣ What does the `math.ceil(pile/mid)` calculation represent in the solution?  
- [ ] A) The number of bananas Koko eats from each pile  
- [ ] B) The hours needed to eat a pile at the current speed (`mid`)  
- [ ] C) The average number of bananas in all piles  
- [ ] D) The remaining bananas in the pile after Koko finishes  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) The hours needed to eat a pile at the current speed (`mid`)**  
</details>  

---

#### 4️⃣ Why do we use `right = mid` (not `right = mid - 1`) when `hours_spent <= h`?  
- [ ] A) To include the current speed as a potential answer since it's valid  
- [ ] B) To avoid an infinite loop in the binary search algorithm  
- [ ] C) Because we're looking for the minimum valid speed, and the current speed might be it  
- [ ] D) Both A and C are correct  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) Both A and C are correct**  
</details>  

---

#### 5️⃣ What is the time complexity of this binary search solution?  
- [ ] A) O(n log n), where n is the number of piles  
- [ ] B) O(n log m), where n is the number of piles and m is the maximum pile size  
- [ ] C) O(h log n), where h is the number of hours and n is the number of piles  
- [ ] D) O(n²), where n is the number of piles  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) O(n log m), where n is the number of piles and m is the maximum pile size**  
</details>  

```python
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        left = 1
        right = max(piles)

        while left < right:
            mid = (left + right) // 2
            hours_spent = 0

            for pile in piles:
                hours_spent += math.ceil(pile/mid)

            if hours_spent <= h:  # Decrease the speed
                right = mid
            
            else:                # Increase the speed
                left = mid + 1
        
        return right
```

# 2300. Successful Pairs of Spells and Potions

### Problem Statement
You are given two positive integer arrays `spells` and `potions`, of length `n` and `m` respectively, where `spells[i]` represents the strength of the `ith` spell and `potions[j]` represents the strength of the `jth` potion.

You are also given an integer `success`. A spell and potion pair is considered **successful** if the **product** of their strengths is **at least** `success`.

Return *an integer array* `pairs` *of length* `n` *where* `pairs[i]` *is the number of* **potions** *that will form a successful pair with the* `ith` *spell.*

### Examples

#### Example 1:
```
Input: spells = [5,1,3], potions = [1,2,3,4,5], success = 7
Output: [4,0,3]
Explanation:
- 0th spell: 5 * [1,2,3,4,5] = [5,10,15,20,25]. 4 pairs are successful.
- 1st spell: 1 * [1,2,3,4,5] = [1,2,3,4,5]. 0 pairs are successful.
- 2nd spell: 3 * [1,2,3,4,5] = [3,6,9,12,15]. 3 pairs are successful.
Thus, [4,0,3] is returned.
```

#### Example 2:
```
Input: spells = [3,1,2], potions = [8,5,8], success = 16
Output: [2,0,2]
Explanation:
- 0th spell: 3 * [8,5,8] = [24,15,24]. 2 pairs are successful.
- 1st spell: 1 * [8,5,8] = [8,5,8]. 0 pairs are successful. 
- 2nd spell: 2 * [8,5,8] = [16,10,16]. 2 pairs are successful. 
Thus, [2,0,2] is returned.
```

#### Constraints:
* `n == spells.length`
* `m == potions.length`
* `1 <= n, m <= 10^5`
* `1 <= spells[i], potions[i] <= 10^5`
* `1 <= success <= 10^10`

### 📌 Multiple Choice Questions: Binary Search Approach  

#### 1️⃣ Why do we sort the `potions` array in this solution?  
- [ ] A) To reduce the time complexity when computing products  
- [ ] B) To enable the use of binary search to find the minimum viable potion  
- [ ] C) To avoid having to sort the results array  
- [ ] D) To make the problem easier to understand  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) To enable the use of binary search to find the minimum viable potion**  
  
</details>  

---

#### 2️⃣ What is the significance of calculating `minPotion = success / spell`?  
- [ ] A) It's the maximum potion strength needed for success  
- [ ] B) It's the minimum potion strength needed for a successful pair with the current spell  
- [ ] C) It determines how many potions to skip in the search  
- [ ] D) It's used to calculate the average success rate  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) It's the minimum potion strength needed for a successful pair with the current spell**  
</details>  

---

#### 3️⃣ Why do we check if `minPotion > maxPotion` before performing binary search?  
- [ ] A) As an optimization to avoid unnecessary searches when no potions can form successful pairs  
- [ ] B) To ensure the binary search doesn't go out of bounds  
- [ ] C) To validate that the input constraints are met  
- [ ] D) To handle edge cases where the spell strength is zero  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) As an optimization to avoid unnecessary searches when no potions can form successful pairs**  
</details>  

---

#### 4️⃣ What does `bisect.bisect_left(potions, minPotion)` return in this solution?  
- [ ] A) The index of the first potion that equals `minPotion`  
- [ ] B) The number of successful pairs for the current spell  
- [ ] C) The index where `minPotion` should be inserted to maintain sorted order  
- [ ] D) The median value of the potions array  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **C) The index where `minPotion` should be inserted to maintain sorted order**  
</details>  

---

#### 5️⃣ What is the time complexity of this solution?  
- [ ] A) O(n × m), where n is the number of spells and m is the number of potions  
- [ ] B) O(n + m log m), where sorting potions takes m log m and iterating through spells takes n  
- [ ] C) O(n × m log m), due to sorting and binary search for each spell  
- [ ] D) O(m log m + n log m), due to sorting potions once and performing binary search for each spell  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) O(m log m + n log m), due to sorting potions once and performing binary search for each spell**  
</details>  

```python
class Solution:
    def successfulPairs(self, spells: List[int], potions: List[int], success: int) -> List[int]:
        res = []
        potions.sort()  # O(m log m)
        
        m = len(potions)
        maxPotion = potions[-1]
        
        for spell in spells:
            minPotion = success / spell
            
            # If minPotion required is more than maxPotion
            if minPotion > maxPotion:
                res.append(0)
                continue
            
            index = bisect.bisect_left(potions, minPotion)
            res.append(m - index)
        
        return res
```

# 162. Find Peak Element

### Problem Statement
A peak element is an element that is strictly greater than its neighbors.

Given a **0-indexed** integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`. In other words, an element is always considered to be strictly greater than a neighbor that is outside the array.

You must write an algorithm that runs in `O(log n)` time.

### Examples

#### Example 1:
```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

#### Example 2:
```
Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
```

#### Constraints:
* `1 <= nums.length <= 1000`
* `-2^31 <= nums[i] <= 2^31 - 1`
* `nums[i] != nums[i + 1]` for all valid `i`.

### 📌 Multiple Choice Questions: Binary Search Approach  

#### 1️⃣ Why is binary search a suitable algorithm for finding a peak element?  
- [ ] A) Because the array is guaranteed to be sorted  
- [ ] B) Because we only need to find any peak, not a specific one  
- [ ] C) Because the problem states we need O(log n) time complexity  
- [ ] D) Both B and C are correct  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) Both B and C are correct**  
  
  Binary search is suitable because we only need to find any peak (not a specific one), and the problem requires O(log n) time complexity. The solution leverages the fact that by comparing adjacent elements, we can determine which direction to search for a peak.
</details>  

---

#### 2️⃣ In the binary search approach, why do we compare `nums[mid]` with `nums[mid + 1]` rather than both neighbors?  
- [ ] A) It's a simplification that still guarantees finding a peak  
- [ ] B) It reduces the number of comparisons needed  
- [ ] C) It helps avoid edge cases at array boundaries  
- [ ] D) All of the above  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) It's a simplification that still guarantees finding a peak**  
  
  By comparing with just the right neighbor, we can determine which half to search. If nums[mid] > nums[mid+1], we know there's a peak on the left side (including mid itself). If nums[mid] < nums[mid+1], there's a peak on the right side.
</details>  

---

#### 3️⃣ When `nums[mid] > nums[mid + 1]`, why do we set `r = mid` (not `r = mid - 1`)?  
- [ ] A) To avoid missing the peak if it's at the current position  
- [ ] B) To handle edge cases when mid is at the array boundaries  
- [ ] C) To ensure the algorithm terminates properly  
- [ ] D) To maintain the O(log n) time complexity  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **A) To avoid missing the peak if it's at the current position**  
  
  When nums[mid] > nums[mid+1], the current position (mid) could be a peak or there might be a peak to its left. Setting r = mid instead of r = mid - 1 ensures we don't exclude mid from our search space.
</details>  

---

#### 4️⃣ What property of the problem guarantees that a peak element always exists?  
- [ ] A) The constraint that `nums[i] != nums[i + 1]` for all valid `i`  
- [ ] B) The assumption that elements outside the array are `-∞`  
- [ ] C) The fact that the array has at least one element  
- [ ] D) Both B and C  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **D) Both B and C**  
  
  Since the array has at least one element and elements outside the array are considered `-∞`, even a single-element array would have a peak (as both its neighbors would be `-∞`). For larger arrays, the fact that adjacent elements cannot be equal (from the constraint) combined with these properties ensures at least one peak exists.
</details>  

---

#### 5️⃣ What is the worst-case time complexity of the binary search approach for finding a peak element?  
- [ ] A) O(n)  
- [ ] B) O(log n)  
- [ ] C) O(n log n)  
- [ ] D) O(1)  

<details>
  <summary>💡 Answer</summary>
  
  ✅ **B) O(log n)**  
  
  The binary search algorithm halves the search space in each iteration, resulting in a logarithmic time complexity of O(log n), where n is the length of the array. This meets the problem's requirement for a O(log n) solution.
</details>  

```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        l = 0
        r = len(nums) - 1
        while l < r:
            mid = (l + r) // 2
            if nums[mid] > nums[mid + 1]:
                r = mid
            else:
                l = mid + 1
        return l
```
