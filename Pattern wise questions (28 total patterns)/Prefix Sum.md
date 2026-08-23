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

---
## 2️⃣ Subarray Sum Equals K

🔗 LeetCode: [https://leetcode.com/problems/subarray-sum-equals-k/](https://leetcode.com/problems/subarray-sum-equals-k/)

---

### Description

Given an array `nums` and integer `k`, return the total number of subarrays whose sum equals `k`.

---

### Core Insight

If `prefixSum[j] - prefixSum[i] = k`, then the subarray between `i+1` and `j` sums to `k`.

So for every current prefix sum, check how many times `(prefixSum - k)` has appeared before — that count tells you how many valid subarrays end here.

```
Prefix Sum + HashMap frequency lookup
```

---

### Algorithm

- `map = {0 → 1}` (empty prefix exists once — handles subarrays starting at index 0)
- `sum = 0`, `count = 0`
- For each `num` in `nums`:
    - `sum += num`
    - `target = sum - k`
    - If `map.containsKey(target)` → `count += map.get(target)`
    - `map[sum]++`
- Return `count`

---

### Code (Java)

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        int sum = 0;
        int count = 0;

        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, 1);

        for (int num : nums) {
            sum += num;

            int target = sum - k;

            if (map.containsKey(target)) {
                count += map.get(target);
            }

            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
```

---

### Example

**Input:** `[1,1,1], k = 2` **Output:** `2`

**Input:** `[1,2,3], k = 3` **Output:** `2` → subarrays `[1,2]` and `[3]`

---

### Complexity

- Time: **O(n)**
- Space: **O(n)**

---

### Interview Notes

Pattern:

```
Prefix Sum + HashMap (Frequency Lookup)
```

Key condition:

```
map.put(0, 1) is critical — without it, subarrays starting at index 0 are never counted
```

---

### Brutal Truth

If you:

- Forget `map.put(0, 1)` → misses all subarrays that start from index 0 and equal `k` directly
- Use `map.containsKey(target)` with `count++` instead of `count += map.get(target)` → undercounts, misses that the same prefix sum can occur multiple times
- Try sliding window instead → fails, array has negative numbers, sum isn't monotonic
- Update `map[sum]` BEFORE checking `target` → could self-match the current element incorrectly in edge cases

---

### Extra Insight

This is the **template problem** for prefix sum + hashmap counting. Compare to Find Pivot Index:

```
Find Pivot Index         → running left sum, DERIVE right via subtraction (no map needed)
Subarray Sum Equals K    → running prefix sum, LOOKUP via hashmap (map needed)
```

Use a map whenever you need to count **how many times** a certain prefix value occurred — not just derive one value from the total.

---
## 3️⃣ Subarray Sums Divisible by K

🔗 LeetCode: [https://leetcode.com/problems/subarray-sums-divisible-by-k/](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

---

### Description

Given an integer array `nums` and integer `k`, return the number of non-empty subarrays whose sum is divisible by `k`.

---

### Core Insight

Same shape as Subarray Sum Equals K — but instead of looking up an exact prefix value, you look up **prefix sums with the same remainder mod k**.

```
If prefixSum[j] % k == prefixSum[i] % k, then subarray (i+1..j) is divisible by k
```

Why? Two prefix sums with the same remainder differ by a multiple of `k` — so their difference (the subarray between them) is divisible by `k`.

---

### Algorithm

- `map = {0 → 1}` (empty prefix has remainder 0, counted once)
- `sum = 0`, `res = 0`
- For each `num` in `nums`:
    - `sum += num`
    - `rem = sum % k`
    - If `rem < 0` → `rem += k` (handle negative remainder)
    - If `map.containsKey(rem)` → `res += map.get(rem)`
    - `map[rem]++`
- Return `res`

---

### Code (Java)

```java
class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        int sum = 0;
        int res = 0;
        HashMap<Integer, Integer> map = new HashMap<>();

        map.put(0, 1);

        for (int num : nums) {
            sum += num;

            int rem = sum % k;
            if (rem < 0) {
                rem += k;
            }

            if (map.containsKey(rem)) {
                res += map.get(rem);
            }

            map.put(rem, map.getOrDefault(rem, 0) + 1);
        }
        return res;
    }
}
```

---

### Example

**Input:** `[4,5,0,-2,-3,1], k = 5` **Output:** `7`

---

### Complexity

- Time: **O(n)**
- Space: **O(k)** — at most `k` distinct remainders

---

### Interview Notes

Pattern:

```
Prefix Sum + HashMap (Remainder Frequency Lookup)
```

Key condition:

```
Group by rem = sum % k instead of exact sum — always normalize negative remainders before lookup
```

---

### Brutal Truth

If you:

- Skip the `rem < 0 → rem += k` fix → Java's `%` can return negative, breaks map lookups entirely (two mathematically equal remainders treated as different keys)
- Forget `map.put(0, 1)` → misses subarrays from index 0 that are already divisible by k
- Confuse this with Subarray Sum Equals K → structurally identical, just map on `remainder` instead of `sum`

---

![[Pasted image 20260824024528.png|616]]

### 📌 NOTE — How to Find Remainder for Negative Numbers (Java)

**The problem:** Java's `%` operator does NOT behave like true mathematical modulo for negative numbers.

```java
-3 % 5   // Java gives -3, NOT 2
```

Mathematically, `-3 mod 5 = 2` (because `-3 = -1×5 + 2`). But Java's `%` is a **remainder operator**, not a modulo operator — it keeps the sign of the dividend.

```
 7 % 5 =  2     (positive stays positive)
-7 % 5 = -2     (Java keeps the negative sign!)
```

**The fix — always normalize:**

```java
int rem = sum % k;
if (rem < 0) {
    rem += k;
}
```

This works because adding `k` to a negative remainder shifts it into the correct positive range `[0, k-1]` without changing which "bucket" it mathematically belongs to.

**One-liner alternative (same effect):**

```java
int rem = ((sum % k) + k) % k;
```

**Rule to remember:**

```
Whenever you compute X % k and X can be negative,
ALWAYS add k and take % k again (or check < 0 and add k) —
otherwise your hashmap buckets will be wrong and silently give incorrect counts.
```

This exact bug is the #1 mistake on this problem — it compiles fine, runs fine, and gives wrong answers only on inputs with negative prefix sums.

---
