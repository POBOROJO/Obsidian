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
## 2️⃣ Minimum Size Subarray Sum

🔗 LeetCode: [https://leetcode.com/problems/minimum-size-subarray-sum/](https://leetcode.com/problems/minimum-size-subarray-sum/)

---

### Description

Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a subarray whose sum is `>= target`. Return `0` if no such subarray exists.

---

### Core Insight

Window size is **not fixed** — shrink from left whenever condition is met.

```
Variable Size Sliding Window
```

Expand right always, shrink left while sum satisfies condition — track minimum length at each valid state.

---

### Algorithm

- `low = 0`, `sum = 0`, `minLen = MAX`
- Expand `high` from `0` to `n-1`:
    - `sum += nums[high]`
    - While `sum >= target`:
        - Update `minLen = min(minLen, high - low + 1)`
        - `sum -= nums[low]`
        - `low++`
- Return `minLen == MAX ? 0 : minLen`

---

### Code (Java)

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int sum = 0;
        int minLen = Integer.MAX_VALUE;
        int low = 0;

        for (int high = 0; high < n; high++) {
            sum += nums[high];

            while (sum >= target) {
                int len = high - low + 1;
                minLen = Math.min(minLen, len);
                sum -= nums[low];
                low++;
            }
        }
        return (minLen == Integer.MAX_VALUE) ? 0 : minLen;
    }
}
```

---

### Example

**Input:** `target = 7, nums = [2,3,1,2,4,3]` **Output:** `2` **Subarray:** `[4,3]`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Variable Size Sliding Window (Shrink on condition)
```

Key condition:

```
All elements positive → sum is monotonic → sliding window works
```

---

### Brutal Truth

If you:

- Use `if` instead of `while` for shrinking → misses smaller valid windows inside current window
- Return `minLen` directly without checking `MAX` → returns garbage when no valid subarray exists
- Try this with negative numbers → sliding window breaks, need prefix sum instead

---
## 3️⃣ Longest Substring with K Uniques

🔗 GFG: [https://www.geeksforgeeks.org/problems/longest-k-unique-characters-substring/1](https://www.geeksforgeeks.org/problems/longest-k-unique-characters-substring/1)

---

### Description

Given a string `s` and integer `k`, find the length of the longest substring that contains **exactly** `k` distinct characters. Return `-1` if no such substring exists.

---

### Core Insight

Variable window + character frequency condition → **Sliding Window + HashMap**.

Expand right always, shrink left when distinct count exceeds `k`, update answer only when distinct count is **exactly** `k`.

---

### Algorithm

- `low = 0`, `res = -1`, `map = {}`
- Expand `high` from `0` to `n-1`:
    - Add `s[high]` to map, increment frequency
    - While `map.size() > k`:
        - Decrement frequency of `s[low]`
        - If frequency hits `0` → remove from map
        - `low++`
    - If `map.size() == k` → update `res = max(res, high - low + 1)`
- Return `res`

---

### Code (Java)

```java
class Solution {
    public int longestKSubstr(String s, int k) {
        int n = s.length();
        int low = 0;
        int res = -1;

        Map<Character, Integer> f = new HashMap<>();

        for (int high = 0; high < n; high++) {
            char c = s.charAt(high);
            f.put(c, f.getOrDefault(c, 0) + 1);

            while (f.size() > k) {
                char leftChar = s.charAt(low);
                f.put(leftChar, f.get(leftChar) - 1);
                if (f.get(leftChar) == 0) {
                    f.remove(leftChar);
                }
                low++;
            }

            if (f.size() == k) {
                res = Math.max(res, high - low + 1);
            }
        }
        return res;
    }
}
```

---

### Example

**Input:** `s = "aabacbebebe", k = 3` **Output:** `7` **Substring:** `"cbebebe"`

---

### Complexity

- Time: **O(n)**
- Space: **O(k)**

---

### Interview Notes

Pattern:

```
Variable Size Sliding Window + HashMap (Exactly K condition)
```

Key condition:

```
Track distinct count via map.size() — shrink when > k, record only when == k
```

---

### Brutal Truth

If you:

- Update `res` inside the `while` loop → window is still invalid there, wrong length
- Use `map.size() >= k` to update res → captures `< k` cases too, wrong answer
- Forget to `remove` key when frequency hits `0` → map size never decreases, window never shrinks correctly

---

## 4️⃣ Fruit Into Baskets

🔗 LeetCode: [https://leetcode.com/problems/fruit-into-baskets/](https://leetcode.com/problems/fruit-into-baskets/)

---

### Description

Given an array `fruits` where `fruits[i]` is the fruit type at tree `i`, find the maximum number of fruits you can pick with exactly **2 baskets** (each basket holds only one fruit type, unlimited quantity).

---

### Core Insight

"2 baskets, each holds 1 type" = **longest subarray with at most 2 distinct values**.

Same pattern as Longest Substring with K Uniques — just `k = 2` hardcoded.

---

### Algorithm

- `low = 0`, `maxLen = 0`, `freq = {}`
- Expand `high` from `0` to `n-1`:
    - Add `fruits[high]` to freq map
    - While `freq.size() > 2`:
        - Decrement freq of `fruits[low]`
        - If freq hits `0` → remove from map
        - `low++`
    - Update `maxLen = max(maxLen, high - low + 1)`
- Return `maxLen`

---

### Code (Java)

```java
class Solution {
    public int totalFruit(int[] fruits) {
        int n = fruits.length;
        int low = 0, maxLen = 0;
        Map<Integer, Integer> freq = new HashMap<>();

        for (int high = 0; high < n; high++) {
            int x = fruits[high];
            freq.put(x, freq.getOrDefault(x, 0) + 1);

            while (freq.size() > 2) {
                int leftFruit = fruits[low];
                freq.put(leftFruit, freq.get(leftFruit) - 1);
                if (freq.get(leftFruit) == 0)
                    freq.remove(leftFruit);
                low++;
            }
            maxLen = Math.max(maxLen, high - low + 1);
        }
        return maxLen;
    }
}
```

---

### Example

**Input:** `[1,2,3,2,2]` **Output:** `4` **Subarray:** `[2,3,2,2]`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)** — map holds at most 3 entries at any time

---

### Interview Notes

Pattern:

```
Variable Size Sliding Window + HashMap (At Most K distinct)
```

Key condition:

```
"At most 2 distinct" → update maxLen OUTSIDE while loop, not inside
```

---

### Brutal Truth

If you:

- Update `maxLen` inside the `while` → window is shrinking there, never a valid max
- Confuse this with "exactly 2" → here `<= 2` is valid, update res whenever window is valid
- Miss that this is just K Uniques with `k = 2` → you'll solve both from scratch instead of recognising the same pattern

---

### Extra Insight

Pattern family comparison:

```
Exactly K distinct   → update res only when map.size() == k  (Longest K Uniques)
At most K distinct   → update res whenever map.size() <= k   (Fruit Into Baskets)
```

Recognising this difference saves you in any variant of this problem.

---