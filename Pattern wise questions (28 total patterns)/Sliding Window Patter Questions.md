### **NOTE ❗❗❗ -   ==When is a question of Sliding Window**== 

1. **When ARRAY OR String**, NOT IN LINKED LIST
2. **[ Subarray / Substring ] -> means they are Continuos and [ Subsequence ] --> this is not cause its non continuous**
3. **Are we finding :**
	1. Maximum 
	2. Minimum 
	3. Longest 
	4. Shortest
	5. Sum / Count / Average 
	6. Atmost K / Atleast K / Exactly K
4. 3 Steps :
	1. Identify Pattern
	2. Window Type -> [Fixed / Varaible]
	3. Data/Info
	4. New Window information nikalo.


## 1️⃣ Max Sum Subarray of Size K

🔗 GFG: [https://www.geeksforgeeks.org/problems/max-sum-subarray-of-size-k5313/1](https://www.geeksforgeeks.org/problems/max-sum-subarray-of-size-k5313/1)

---

### Description

Given an array `arr[]` and a number `k`, return the maximum sum of any contiguous subarray of size exactly `k`.

---

### Core Insight

Fixed window size → classic **Sliding Window**.

Don't recompute the sum from scratch each time. Just:

```
remove leftmost element + add new rightmost element
```

---

### Algorithm

- Compute sum of first `k` elements → `res = sum`
- Slide window from `i = k` to `n-1`:
    - `sum -= arr[i - k]` (remove left)
    - `sum += arr[i]` (add right)
    - Update `res = max(res, sum)`
- Return `res`

---

### Code (Java)

```java
class Solution {
    public int maxSubarraySum(int[] arr, int k) {
        int n = arr.length;
        int sum = 0;

        for (int i = 0; i < k; i++) {
            sum += arr[i];
        }

        int res = sum;

        for (int i = k; i < n; i++) {
            sum -= arr[i - k];
            sum += arr[i];
            res = Math.max(res, sum);
        }

        return res;
    }
}
```

---

### Example

**Input:** `[1,4,2,10,23,3,1,0,20], k = 4` **Output:** `39` **Window:** `[4,2,10,23]`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Fixed Size Sliding Window
```

Key condition:

```
Window size is fixed → use sliding window, not recomputation
```

---

### Brutal Truth

If you:

- Recompute sum from scratch each window → O(n*k), fails for large inputs
- Start second loop from `i = 1` instead of `i = k` → wrong index for removal
- Remove `arr[i]` instead of `arr[i-k]` → removing wrong element, window corrupted

---