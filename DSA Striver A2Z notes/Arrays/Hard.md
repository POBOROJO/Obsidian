---
tags:
  - arrays
  - hard
---

## ✅ Problem Tracker
- [x] 1️⃣7️⃣ Pascal's Triangle
- [x] 1️⃣8️⃣ Majority Element II (N/3 times)
- [x] 1️⃣9️⃣ 3Sum
- [x] 2️⃣0️⃣ 4Sum
- [x] 2️⃣1️⃣ Largest Subarray with 0 Sum
- [ ] 2️⃣2️⃣ Count Subarrays with XOR = K
- [ ] 2️⃣3️⃣ Merge Overlapping Intervals
- [ ] 2️⃣4️⃣ Merge Two Sorted Arrays Without Extra Space
- [ ] 2️⃣5️⃣ Find Missing and Repeating Number
- [ ] 2️⃣6️⃣ Count Inversions (Merge Sort)
- [ ] 2️⃣7️⃣ Reverse Pairs (Merge Sort)
- [ ] 2️⃣8️⃣ Maximum Product Subarray

# 1️⃣7️⃣ Pascal’s Triangle

### Problem

Given an integer `numRows`, return the first `numRows` of Pascal’s Triangle.
Each number is the sum of the two numbers directly above it.

---

### Core Insight

Each row follows **nCr (binomial coefficients)**.
Next value can be computed from previous using:

$$
C = C \times \frac{(row - col)}{col}
$$

Avoids recomputing sums and stays efficient.

---

### Algorithm

For each row `r` from `1 → numRows`:

* Start with `1`
* Use formula to generate next elements
* Add row to result

No need to look at previous rows.

---

### Code (Java)

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> allRows = new ArrayList<>();

        for (int row = 1; row <= numRows; row++) {
            List<Integer> current = new ArrayList<>();
            long val = 1;

            current.add((int) val);

            for (int col = 1; col < row; col++) {
                val = val * (row - col);
                val = val / col;
                current.add((int) val);
            }

            allRows.add(current);
        }
        return allRows;
    }
}
```

---

### Example

```
            1
          1   1
        1   2   1
      1   3   3   1
    1   4   6   4   1
  1   5  10  10   5   1
```

Input: `5`
Output: `[[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]`

---

### Complexity

* Time: **O(n²)**
* Space: **O(n²)** (output storage)

---

### Interview Notes

* Uses combinatorics instead of DP
* `long` prevents overflow
* First and last elements are always `1`
* Pattern: **Combinatorics / Binomial Coefficients**

---

# 1️⃣8️⃣ Majority Element II (n/3 Version)

### Problem

Given an array of size `n`, return all elements that appear more than ⌊n/3⌋ times.

Follow-up: Solve in **O(n)** time and **O(1)** space.

---

### Core Insight

More than ⌊n/3⌋ ⇒ **at most 2 such elements** can exist.
So track **two candidates** and cancel others.

> If 3 numbers each appeared > n/3 → total > n → impossible.

---

### Algorithm

Phase 1: Find candidates

* Maintain `(el1,cnt1)` and `(el2,cnt2)`
* If number matches candidate → increment
* If count becomes 0 → assign new candidate
* Otherwise decrement both

Phase 2: Verify

* Count occurrences
* Add elements > n/3

---

### Code (Java)

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        int cnt1 = 0, cnt2 = 0;
        int el1 = 0, el2 = 0;

        for (int x : nums) {
            if (el1 == x) cnt1++;
            else if (el2 == x) cnt2++;
            else if (cnt1 == 0) { el1 = x; cnt1 = 1; }
            else if (cnt2 == 0) { el2 = x; cnt2 = 1; }
            else { cnt1--; cnt2--; }
        }

        cnt1 = 0; cnt2 = 0;
        for (int x : nums) {
            if (x == el1) cnt1++;
            else if (x == el2) cnt2++;
        }

        List<Integer> ans = new ArrayList<>();
        int limit = nums.length / 3;

        if (cnt1 > limit) ans.add(el1);
        if (cnt2 > limit) ans.add(el2);

        return ans;
    }
}
```

---

### Example

Input: `[1,2,1,2,1,3]`
Output: `[1]`

---

### Complexity

* Time: **O(n)**
* Space: **O(1)**

---

### Interview Notes

* Extension of Boyer–Moore
* Maximum 2 valid answers
* Verification pass required
* Pattern: **Extended Boyer–Moore Voting**

---

# 1️⃣9️⃣ 3Sum

### Problem

Given an integer array `nums`, return all unique triplets `[a,b,c]` such that:

```
a + b + c = 0
```

Triplets must be unique.

---

### Core Insight

Fix one element, then reduce to **Two Sum on a sorted array**.

Sorting enables:

* Duplicate skipping
* Efficient pointer movement

---

### Algorithm

1. Sort the array
2. For each index `i`:

   * Skip duplicates
   * Use two pointers `j = i+1`, `k = n-1`
3. While `j < k`:

   * If sum < 0 → `j++`
   * If sum > 0 → `k--`
   * If sum == 0:

     * Add triplet
     * Move both
     * Skip duplicates

---

### Code (Java)

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;

            int j = i + 1, k = n - 1;

            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];

                if (sum < 0) j++;
                else if (sum > 0) k--;
                else {
                    ans.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    j++; k--;

                    while (j < k && nums[j] == nums[j - 1]) j++;
                    while (j < k && nums[k] == nums[k + 1]) k--;
                }
            }
        }
        return ans;
    }
}
```

---

### Example

Input: `[-1,0,1,2,-1,-4]`
Output: `[[-1,-1,2],[-1,0,1]]`

---

### Complexity

* Time: **O(n²)**
* Space: **O(1)** (excluding output)

---

### Interview Notes

* Sorting mandatory
* Skip duplicates at all levels
* Optimal complexity is O(n²)
* Pattern: **Sorting + Two Pointers**

---

# 2️⃣0️⃣ 4Sum

### Problem

Given array `nums` and integer `target`, return all unique quadruplets `[a,b,c,d]` such that:

```
a + b + c + d = target
```

---

### Core Insight

Fix two elements → reduce to **Two Pointer search**.
Extension of 3Sum.

---

### Algorithm

1. Sort array
2. Fix `i`

   * Skip duplicates
3. Fix `j`

   * Skip duplicates
4. Use two pointers:

   * `k = j+1`
   * `l = n-1`
5. Compare sum:

   * < target → `k++`
   * > target → `l--`
   * == target → add & skip duplicates

---

### Code (Java)

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;

            for (int j = i + 1; j < n; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;

                int k = j + 1, l = n - 1;

                while (k < l) {
                    long sum = (long) nums[i] + nums[j] + nums[k] + nums[l];

                    if (sum < target) k++;
                    else if (sum > target) l--;
                    else {
                        ans.add(Arrays.asList(nums[i], nums[j], nums[k], nums[l]));
                        k++; l--;

                        while (k < l && nums[k] == nums[k - 1]) k++;
                        while (k < l && nums[l] == nums[l + 1]) l--;
                    }
                }
            }
        }
        return ans;
    }
}
```

---

### Example

Input: `[1,0,-1,0,-2,2]`, `target = 0
Output: `[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]`

---

### Complexity

* Time: **O(n³)**
* Space: **O(1)** (excluding output)

---

### Interview Notes

* Use `long` to avoid overflow
* Skip duplicates at all levels
* Natural extension: 2Sum → 3Sum → 4Sum → kSum
* Pattern: **Sorting + Nested Two Pointers**

---

Good. You’re right.  
From now on: **Description + Core Insight + Algorithm + Code + Example + Complexity + Interview Notes.**

Here’s the updated version for:

---

## 2️⃣1️⃣ Largest Subarray with 0 Sum

### Description

Given an array containing positive and negative integers, find the **length of the longest contiguous subarray** whose sum is equal to `0`.

A subarray must be continuous (not a subset).

---

### Core Insight

If two prefix sums are equal at indices `i` and `j`,  
then the subarray between them has **sum = 0**.

Why?  
Because:

```
prefix[j] - prefix[i] = 0
```

So store the **first occurrence** of each prefix sum.

---

### Algorithm

- Initialize:
    
    - `sum = 0`
        
    - `maxi = 0`
        
    - `HashMap<sum, firstIndex>`
        
- Traverse array:
    
    - Add current element to `sum`
        
    - If `sum == 0` → update `maxi = i+1`
        
    - If `sum` already seen:
        
        - Length = `i - firstIndex`
            
        - Update `maxi`
            
    - Else:
        
        - Store `sum → i` (first occurrence only)
            
- Return `maxi`
    

---

### Code (Java)

```java
class Solution {
    int maxLength(int arr[]) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int sum = 0, maxi = 0;

        for (int i = 0; i < arr.length; i++) {
            sum += arr[i];

            if (sum == 0) {
                maxi = i + 1;
            } 
            else if (map.containsKey(sum)) {
                maxi = Math.max(maxi, i - map.get(sum));
            } 
            else {
                map.put(sum, i); // store first occurrence only
            }
        }
        return maxi;
    }
}
```

---

### Example

**Input:**  
`[15, -2, 2, -8, 1, 7, 10, 23]`

**Output:**  
`5`

Subarray: `[-2, 2, -8, 1, 7]`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(n)**
    

---

### Interview Notes

- Works because negatives are allowed → sliding window fails
    
- Must store **first occurrence only**
    
- Prefix sum collision = zero-sum subarray
    
- Pattern: **Prefix Sum + HashMap**
---
