![[Pasted image 20260808160513.png]]

## **NOTE ❗❗❗ — When is a question of Kadane's Algorithm**

**Tags: Dynamic Programming / Greedy on Subarrays**

1. **Maximum / Minimum subarray sum**
2. **Contiguous subarray + optimize some value**
3. **"Find subarray" + no extra space**
4. **Running optimal that can reset**
---

## 1️⃣ Maximum Subarray

🔗 LeetCode: [https://leetcode.com/problems/maximum-subarray/](https://leetcode.com/problems/maximum-subarray/)

---

### Description

Given an integer array `nums`, find the subarray with the largest sum and return its sum.

---

### Core Insight

At every index, you have exactly **two choices**:

```
1. Extend the previous subarray → bestEnding + nums[i]
2. Start fresh from current element → nums[i]
```

Pick whichever is larger. A negative running sum is dead weight — starting fresh is always better.

---

### Algorithm

- `bestEnding = nums[0]`, `ans = nums[0]`
- From index `1` onward:
    - `v1 = bestEnding + nums[i]` (extend)
    - `v2 = nums[i]` (start fresh)
    - `bestEnding = max(v1, v2)`
    - `ans = max(ans, bestEnding)`
- Return `ans`

---

### Code (Java)

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int bestEnding = nums[0];
        int ans = nums[0];

        for (int i = 1; i < nums.length; i++) {
            int v1 = bestEnding + nums[i];
            int v2 = nums[i];
            bestEnding = Math.max(v1, v2);

            ans = Math.max(bestEnding, ans);
        }
        return ans;
    }
}
```

---

### Example

**Input:** `[-2,1,-3,4,-1,2,1,-5,4]`

```
i=0: bestEnding=-2,  ans=-2
i=1: max(-2+1, 1)=1, ans=1
i=2: max(1-3, -3)=-2, ans=1
i=3: max(-2+4, 4)=4, ans=4
i=4: max(4-1, -1)=3, ans=4
i=5: max(3+2, 2)=5, ans=5
i=6: max(5+1, 1)=6, ans=6
i=7: max(6-5, -5)=1, ans=6
i=8: max(1+4, 4)=5, ans=6
```

**Output:** `6` → subarray `[4,-1,2,1]`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Kadane's Algorithm (Extend or Reset)
```

Key condition:

```
bestEnding = max(extend, restart) — this single line IS Kadane's
```

---

### Brutal Truth

If you:

- Initialize `ans = 0` → wrong for all-negative arrays, returns `0` instead of the least negative element
- Update `ans` before `bestEnding` → captures stale value
- Use `sum < 0 → reset to 0` style → same algorithm, but breaks for all-negative arrays unless you init `ans = nums[0]`
- Try divide and conquer → O(n log n), correct but overkill for this problem

---

### ALGO TO REMEMBER — Kadane's Algorithm

**What it does:** finds maximum sum contiguous subarray in O(n), O(1)

**Hook:** _"At every step ask — is it better to continue my journey or start a new one from here?"_

**Core logic — just two lines:**

```
bestEnding = max(bestEnding + current, current)
ans        = max(ans, bestEnding)
```

**The reset happens automatically** — when `current > bestEnding + current`, that means `bestEnding` was negative, so you drop it and start fresh. No explicit reset needed.

---

## 2️⃣ Minimum Sum Subarray

🔗 GFG: [https://www.geeksforgeeks.org/problems/smallest-sum-subarray1624/1](https://www.geeksforgeeks.org/problems/smallest-sum-subarray1624/1)

---

### Description

Given an array `arr[]`, find the subarray with the minimum sum (must contain at least one element) and return its sum.

---

### Core Insight

Exact mirror of Kadane's Maximum Subarray — just flip `max` to `min`.

```
Extend if it makes sum smaller, restart if current alone is smaller
```

---

### Algorithm

- `bestEnding = a[0]`, `ans = a[0]`
- From index `1` onward:
    - `v1 = bestEnding + a[i]` (extend)
    - `v2 = a[i]` (start fresh)
    - `bestEnding = min(v1, v2)`
    - `ans = min(ans, bestEnding)`
- Return `ans`

---

### Code (Java)

```java
class Solution {
    static int smallestSumSubarray(int a[], int size) {
        int bestEnding = a[0];
        int ans = a[0];

        for (int i = 1; i < size; i++) {
            int v1 = bestEnding + a[i];
            int v2 = a[i];
            bestEnding = Math.min(v1, v2);
            ans = Math.min(bestEnding, ans);
        }
        return ans;
    }
}
```

---

### Example

**Input:** `[3,-4,2,-3,-1,7,-5]`

```
i=0: bestEnding=3,   ans=3
i=1: min(3-4,-4)=-4, ans=-4
i=2: min(-4+2,2)=-2, ans=-4
i=3: min(-2-3,-3)=-5, ans=-5
i=4: min(-5-1,-1)=-6, ans=-6
i=5: min(-6+7,7)=1,  ans=-6
i=6: min(1-5,-5)=-4, ans=-6
```

**Output:** `-6` → subarray `[-4,2,-3,-1]`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Kadane's Algorithm (Minimization variant)
```

Key condition:

```
bestEnding = min(extend, restart) — same logic, opposite direction
```

---

### Brutal Truth

If you:

- Initialize `ans = 0` → wrong for all-positive arrays, returns `0` instead of the smallest element
- Use `max` instead of `min` anywhere → solving the wrong problem
- Forget this is just Kadane's flipped → waste time re-deriving from scratch

---

### Extra Insight

Kadane's family so far:

```
Maximum Subarray  → bestEnding = max(extend, restart), ans = max
Minimum Subarray  → bestEnding = min(extend, restart), ans = min
```

The template never changes — only the comparison operator flips. Every Kadane's variant follows this same two-line core:

```
bestEnding = op(bestEnding + current, current)
ans        = op(ans, bestEnding)
```

Where `op` is `max` or `min` depending on what you're optimizing.

---
## 3️⃣ Maximum Product Subarray

🔗 LeetCode: [https://leetcode.com/problems/maximum-product-subarray/](https://leetcode.com/problems/maximum-product-subarray/)

---

### Description

Given an integer array `nums`, find the subarray with the largest product and return that product.

---

### Core Insight

Product behaves differently from sum — a negative number can **flip** the smallest product into the largest.

So simple Kadane's (track only max) fails. You must track **both max AND min** ending at each position, because a negative multiplied by the current min could become the new max.

```
This is NOT the same pattern as Max/Min Subarray Sum — it's a new sub-pattern
```

---

### Algorithm

- `minEnding = arr[0]`, `maxEnding = arr[0]`, `res = arr[0]`
- From index `1` onward:
    - `v1 = arr[i]` (start fresh)
    - `v2 = minEnding * arr[i]` (extend via min — useful if arr[i] is negative)
    - `v3 = maxEnding * arr[i]` (extend via max)
    - `maxEnding = max(v1, v2, v3)`
    - `minEnding = min(v1, v2, v3)`
    - `res = max(res, maxEnding)`
- Return `res`

---

### Code (Java)

```java
class Solution {
    public int maxProduct(int[] arr) {
        int minEnding = arr[0];
        int maxEnding = arr[0];
        int res = arr[0];

        for (int i = 1; i < arr.length; i++) {
            int v1 = arr[i];
            int v2 = minEnding * arr[i];
            int v3 = maxEnding * arr[i];

            maxEnding = Math.max(v1, Math.max(v2, v3));
            minEnding = Math.min(v1, Math.min(v2, v3));
            res = Math.max(res, Math.max(maxEnding, minEnding));
        }
        return res;
    }
}
```

---

### Example

**Input:** `[2,3,-2,4]`

```
i=0: minEnding=2,  maxEnding=2,  res=2
i=1: v1=3, v2=6, v3=6  → max=6, min=3,  res=6
i=2: v1=-2, v2=-6, v3=-12 → max=-2, min=-12, res=6
i=3: v1=4, v2=-48, v3=-8  → max=4, min=-48, res=6
```

**Output:** `6` → subarray `[2,3]`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Kadane's Variant — Track Max AND Min (Product flips on negative)
```

Key condition:

```
Compute all 3 candidates (v1, v2, v3) BEFORE updating maxEnding/minEnding — 
using the already-updated maxEnding while computing minEnding corrupts the result
```

---

### Brutal Truth

If you:

- Track only max (standard Kadane's) → fails immediately, misses the negative-flip case
- Update `maxEnding` first then use it to compute `minEnding` → uses corrupted value, classic bug
- Forget `v1 = arr[i]` (the restart option) → misses cases where zero breaks the subarray, must restart fresh
- Try prefix+suffix approach (your earlier solution) → also valid, different technique, but this min/max tracking is the more "textbook" DP approach interviewers expect

---

### Extra Insight

This is a **distinct sub-pattern** under Kadane's topic — not the same as max/min sum:

```
Max/Min Subarray SUM      → track ONE running value (extend or restart)
Max Product Subarray      → track TWO running values (max AND min, because negative flips them)
```

Whenever a problem involves **multiplication** and the array can have **negative numbers**, your brain should immediately think: _"I need both max and min trackers, not just one."_