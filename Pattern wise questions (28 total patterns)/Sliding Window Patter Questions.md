### **NOTE ❗❗❗ -   ==When is a question of Sliding Window**== 


---
---
tags:
  - subarray
  - substring
  - sliding window
---
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
# FIXED SIZE WINDOW TEMPLATE : 

![[Pasted image 20260609230321.png]]

### The max above is for if there is a scenario if we need to find the max



# VARIABLE SIZE WINDOW TEMPLATE : 

![[Pasted image 20260610004209.png]]



# MINIMUM WINDOW QUESTION : 

![[Pasted image 20260610021354.png]]


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
## 5️⃣ Longest Substring Without Repeating Characters

🔗 LeetCode: [https://leetcode.com/problems/longest-substring-without-repeating-characters/](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

---

### Description

Given a string `s`, find the length of the longest substring without any duplicate characters.

---

### Core Insight

No duplicates = all characters in window must be unique = `map.size() == window length`.

Shrink from left whenever a duplicate enters the window.

---

### Algorithm

- `low = 0`, `res = 0`, `freq = {}`
- Expand `high` from `0` to `n-1`:
    - Add `s[high]` to freq map
    - While `freq.size() < window size` (duplicate exists):
        - Decrement freq of `s[low]`
        - If freq hits `0` → remove from map
        - `low++`
    - Update `res = max(res, high - low + 1)`
- Return `res`

---

### Code (Java)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int res = 0;
        int low = 0;
        Map<Character, Integer> f = new HashMap<>();

        for (int high = 0; high < n; high++) {
            char c = s.charAt(high);
            f.put(c, f.getOrDefault(c, 0) + 1);

            int k = high - low + 1;

            while (f.size() < k) {
                char leftChar = s.charAt(low);
                f.put(leftChar, f.get(leftChar) - 1);
                if (f.get(leftChar) == 0) {
                    f.remove(leftChar);
                }
                low++;
                k = high - low + 1;
            }
            res = Math.max(res, k);
        }
        return res;
    }
}
```

---

### Example

**Input:** `"abcabcbb"` **Output:** `3` **Substring:** `"abc"`

---

### Complexity

- Time: **O(n)**
- Space: **O(min(n, 26))**

---

### Interview Notes

Pattern:

```
Variable Size Sliding Window (All Unique condition)
```

Key condition:

```
No duplicates = map.size() must equal window length — shrink whenever they diverge
```

---

### Brutal Truth

If you:

- Initialize `res = Integer.MIN_VALUE` → unnecessary, no valid window returns `MIN_VALUE` not `0`, just init to `0`
- Forget to update `k` inside the `while` loop → stale window size, wrong shrink condition
- Use a `Set` instead of map → cleaner actually, `set.size() == window length` is the same check with less code

---

### Extra Insight

Cleaner alternative using `Set`:

```java
Set<Character> set = new HashSet<>();
// shrink when set already contains s[high]
// set naturally has no duplicates — no size comparison needed
```

Condition becomes: if `set.contains(s[high])` → shrink until removed, then add. Easier to think about in interviews.

---
## 6️⃣ Subarray Product Less Than K

🔗 LeetCode: [https://leetcode.com/problems/subarray-product-less-than-k/](https://leetcode.com/problems/subarray-product-less-than-k/)
🔗 GFG: https://www.geeksforgeeks.org/problems/count-the-subarrays-having-product-less-than-k1708/1

---

### Description

Given an array of integers `nums` and integer `k`, return the count of contiguous subarrays where the product of all elements is **strictly less than** `k`.

---

### Core Insight

For every valid window ending at `high`, the number of new valid subarrays added is exactly `high - low + 1`.

Why? Every subarray ending at `high` and starting anywhere from `low` to `high` is valid.

---

### Algorithm

- `low = 0`, `product = 1`, `count = 0`
- Edge case: if `k <= 1` → return `0` immediately
- Expand `high` from `0` to `n-1`:
    - `product *= nums[high]`
    - While `product >= k`:
        - `product /= nums[low]`
        - `low++`
    - `count += high - low + 1`
- Return `count`

---

### Code (Java)

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int n = nums.length;
        int count = 0;
        int product = 1;
        int low = 0;

        if (k <= 1) return 0;

        for (int high = 0; high < n; high++) {
            product *= nums[high];

            while (product >= k) {
                product /= nums[low];
                low++;
            }
            count += high - low + 1;
        }
        return count;
    }
}
```

---

### Example

**Input:** `[10,5,2,6], k = 100` **Output:** `8`

Valid subarrays: `[10],[5],[2],[6],[10,5],[5,2],[2,6],[5,2,6]`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Variable Size Sliding Window (Count subarrays via window size)
```

Key condition:

```
Each valid window of size (high - low + 1) contributes exactly that many new subarrays
```

---

### Brutal Truth

If you:

- Miss `k <= 1` edge case → `product >= k` never becomes false when `k = 0` or `k = 1`, infinite loop
- Count `1` per window instead of `high - low + 1` → undercounts, misses all sub-windows inside current window
- Try prefix sum approach → product doesn't have the clean subtraction property that sum has, division works only because all elements are positive

---

### Extra Insight

The counting trick `high - low + 1` appears whenever:

```
every subarray ending at high and starting at [low..high] is valid
```

You'll see this same trick in:

- Count subarrays with sum <= k
- Count subarrays with at most k distinct

Recognise the shape, not just this problem.

---
## 7️⃣ Longest Repeating Character Replacement

🔗 LeetCode: [https://leetcode.com/problems/longest-repeating-character-replacement/](https://leetcode.com/problems/longest-repeating-character-replacement/) 🔗 GFG: [https://www.geeksforgeeks.org/problems/longest-repeating-character-replacement/1](https://www.geeksforgeeks.org/problems/longest-repeating-character-replacement/1)

---

### Description

Given a string `s` and integer `k`, you can replace any character at most `k` times. Return the length of the longest substring containing the same letter after at most `k` replacements.

---

### Core Insight

In any valid window, characters to replace = `window size - count of most frequent character`.

If `(len - maxCount) <= k` → window is valid.

---

### Algorithm

- `low = 0`, `res = 0`, `freq[256] = {}`
- Expand `high` from `0` to `n-1`:
    - Increment `freq[s[high]]`
    - Compute `maxCount = max frequency in window`
    - While `(len - maxCount) > k`:
        - Decrement `freq[s[low]]`
        - `low++`
        - Recompute `maxCount` and `len`
    - Update `res = max(res, high - low + 1)`
- Return `res`

---

### Code (Java)

```java
class Solution {

    public int find(int[] a) {
        int maxc = -1;
        for (int i = 0; i < 256; i++) {
            maxc = Math.max(maxc, a[i]);
        }
        return maxc;
    }

    public int characterReplacement(String s, int k) {
        int n = s.length();
        int res = 0;
        int[] f = new int[256];
        int low = 0;

        for (int high = 0; high < n; high++) {
            f[s.charAt(high)]++;
            int maxCnt = find(f);
            int len = high - low + 1;
            int diff = len - maxCnt;

            while (diff > k) {
                f[s.charAt(low)]--;
                low++;
                maxCnt = find(f);
                len = high - low + 1;
                diff = len - maxCnt;
            }

            res = Math.max(res, high - low + 1);
        }
        return res;
    }
}
```

---

### Example

**Input:** `s = "AABABBA", k = 1` **Output:** `4` **Substring:** `"AABA"` or `"ABBB"` after replacement

---

### Complexity

- Time: **O(n × 256)** → effectively **O(n)**
- Space: **O(256)** → **O(1)**

---

### Interview Notes

Pattern:

```
Variable Size Sliding Window (Replacement budget condition)
```

Key condition:

```
window valid when: (window size - maxFrequency) <= k
```

---

### Brutal Truth

If you:

- Initialize `res = Integer.MIN_VALUE` → unnecessary, just use `0`
- Skip recomputing `maxCnt` inside `while` → stale max causes window to shrink incorrectly
- Use `HashMap` instead of `int[256]` → works but slower, array is cleaner here since input is uppercase only
- Forget that `find()` runs 256 iterations every step → for strict O(n) optimization, track `maxCount` as a running variable instead of rescanning

---

### Extra Insight

Optimized `maxCount` tracking — no need to rescan array every time:

```java
maxCount = Math.max(maxCount, f[s.charAt(high)]);
```

Only update upward on expand. On shrink, `maxCount` may be stale but it never causes wrong answers — window only grows when a genuinely better max is found. This brings it to true **O(n)**.

---

## 8️⃣ Minimum Window Substring

🔗 LeetCode: [https://leetcode.com/problems/minimum-window-substring/](https://leetcode.com/problems/minimum-window-substring/)

---

### Description

Given strings `s` and `t`, return the minimum window substring of `s` that contains every character of `t` (including duplicates). Return `""` if no such window exists.

---

### Core Insight

Expand until window is valid (contains all of `t`), then shrink from left to minimize — track the smallest valid window seen.

Two frequency arrays: `need` (from `t`) and `have` (current window). Window is valid when `have[i] >= need[i]` for all characters.

---

### Algorithm

- Build `need[]` from `t`
- `low = 0`, `res = MAX`, `start = -1`
- Expand `high` from `0` to `n-1`:
    - Increment `have[s[high]]`
    - While window is valid (`have >= need` for all chars):
        - Update `res` and `start` if current window is smaller
        - Decrement `have[s[low]]`
        - `low++`
- Return `res == MAX ? "" : s.substring(start, start + res)`

---

### Code (Java)

```java
class Solution {

    private boolean fun(int[] have, int[] need) {
        for (int i = 0; i < 256; i++) {
            if (have[i] < need[i]) return false;
        }
        return true;
    }

    public String minWindow(String s, String t) {
        int n = s.length();
        int m = t.length();
        int res = Integer.MAX_VALUE;
        int start = -1;
        int low = 0;

        if (m > n) return "";

        int[] need = new int[256];
        int[] have = new int[256];

        for (int i = 0; i < m; i++) {
            need[t.charAt(i)]++;
        }

        for (int high = 0; high < n; high++) {
            have[s.charAt(high)]++;

            while (fun(have, need)) {
                int len = high - low + 1;
                if (len < res) {
                    res = len;
                    start = low;
                }
                have[s.charAt(low)]--;
                low++;
            }
        }
        return res == Integer.MAX_VALUE ? "" : s.substring(start, start + res);
    }
}
```

---

### Example

**Input:** `s = "ADOBECODEBANC", t = "ABC"` **Output:** `"BANC"`

---

### Complexity

- Time: **O(n × 256)** → effectively **O(n)**
- Space: **O(256)** → **O(1)**

---

### Interview Notes

Pattern:

```
Variable Size Sliding Window (Shrink on valid, minimize window)
```

Key condition:

```
Shrink while valid — opposite of most sliding window problems where you shrink while invalid
```

---

### Brutal Truth

If you:

- Shrink while invalid instead of while valid → never finds minimum, wrong approach entirely
- Forget to track `start` index separately → can't reconstruct the substring even if length is right
- Call `fun()` every iteration with 256 loop → works but O(256n); optimized version tracks a `formed` counter instead of scanning full array each time
- Return `s.substring(start, start + res)` when `start = -1` → crash, always guard with `res == MAX` check first

---

### Extra Insight

Optimized approach avoids the 256-scan using a `formed` counter:

```
formed = number of unique chars in t whose frequency is satisfied in window
when formed == required (unique chars in t) → window is valid
```

Increment `formed` when `have[c] == need[c]`, decrement when it drops below. Brings it to true **O(n)**.

---