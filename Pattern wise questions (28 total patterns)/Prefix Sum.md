![[Pasted image 20260820005524.png]]


## **NOTE ❗❗❗ — When is a question of Prefix Sum**

**Tags: Running Total / Cumulative Sum**

1. **Subarray sum queries**
2. **Left sum == right sum / balance point**
3. **Range sum computed repeatedly → precompute once**
4. **Sum-based comparison across a split point**
---

## 1️⃣ Find Pivot Index / Find the Middle Index in Array

🔗 LeetCode: [https://leetcode.com/problems/find-pivot-index/](https://leetcode.com/problems/find-pivot-index/) 
🔗 Same as: [1991. Find the Middle Index in Array](https://leetcode.com/problems/find-the-middle-index-in-array/)

---

### Description

Given an array `nums`, find the leftmost index where the sum of elements strictly to the left equals the sum of elements strictly to the right. Return `-1` if none exists.

---

### Core Insight

Instead of recomputing left and right sums at every index (`O(n²)`), compute `totalSum` once. Then as you scan left to right, maintain a running `left` sum — `right` is derived instantly:

```
right = totalSum - nums[i] - left
```

---

### Algorithm

- Compute `sum` = total of array
- `left = 0`
- Special case: check index `0` first (`sum - nums[0] == 0`)
- For `i` from `1` to `n-1`:
    - `left += nums[i-1]` (accumulate everything before current index)
    - `right = sum - nums[i] - left`
    - If `right == left` → return `i`
- Return `-1`

---

### Code (Java)

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int sum = 0;

        for (int i : nums) {
            sum += i;
        }

        int left = 0;
        if (sum - nums[0] == 0) return 0;

        for (int i = 1; i < nums.length; i++) {
            left += nums[i - 1];
            int right = sum - nums[i] - left;
            if (right == left) return i;
        }
        return -1;
    }
}
```

---

### Example

**Input:** `[1,7,3,6,5,6]` **Output:** `3` **Explanation:** left = `1+7+3=11`, right = `5+6=11`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Prefix Sum (Running Left Sum, Derived Right Sum)
```

Key condition:

```
right = totalSum - nums[i] - left — no need to store a separate prefix array,
one running variable is enough
```

---

### Brutal Truth

If you:

- Recompute left and right sums inside the loop by re-summing slices → O(n²), fails on large inputs
- Forget the index `0` special case → `left` starts at `0` correctly but you must check it BEFORE the loop since the loop starts at `i=1`
- Update `left` with `nums[i]` instead of `nums[i-1]` → off-by-one, includes current element in left sum which violates "strictly left"
- Build an actual prefix sum array when only a running variable is needed → wastes O(n) space unnecessarily here

---

### ALGO TO REMEMBER — Prefix Sum (Running Total)

**What it does:** avoids recomputing range sums repeatedly by maintaining one running cumulative value

**Hook:** _"Don't re-add what you've already added — just remember the running total and derive the rest by subtraction"_

**Core steps:**

1. Compute total sum once
2. Walk through array, maintaining `left` (or prefix) as you go
3. Derive whatever you need (`right`, or `target - k`) via subtraction — never recompute from scratch

---

![[Pasted image 20260819031213.png]]