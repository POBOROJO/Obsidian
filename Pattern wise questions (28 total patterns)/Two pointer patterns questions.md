### **NOTE ❗❗❗ -   ==When is a question of 2 pointer**== 

---
tags:
  - two pointers
---

1. **When ARRAY OR Linked List** 
2. **Sorted  / Can be easy if sorted** 
3. **Merge / Remove Duplicate / Rearrange / in place / Detach Cycle**
4. **Find Pair / Triplets / Quadruples / Or more**

## 1️⃣ Two Sum II – Input Array Is Sorted

🔗 LeetCode: [https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

---

### Description

Given a **1-indexed sorted array** `numbers`, find two elements such that:

```java
numbers[index1] + numbers[index2] = target
```

Constraints:

- `1 <= index1 < index2 <= n`
    
- Exactly **one solution exists**
    
- You **cannot reuse the same element**
    
- Must use **O(1) space**
    

---

### Core Insight

Sorted array → this is screaming:

```java
Two Pointers
```

Why?

- If sum is too big → move right pointer left
    
- If sum is too small → move left pointer right
    

No need for HashMap. If you used it, you ignored the main hint.

---

### Algorithm

- Initialize:
    
    - `i = 0` (start)
        
    - `j = n - 1` (end)
        
- Loop while `i < j`:
    
    - If `nums[i] + nums[j] == target` → return answer
        
    - If sum > target → decrease `j`
        
    - If sum < target → increase `i`
        

---

### Code (Java)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        int i = 0;
        int j = n - 1;

        while (i < j) {
            int sum = nums[i] + nums[j];

            if (sum == target) {
                return new int[]{i + 1, j + 1};
            } 
            else if (sum > target) {
                j--;
            } 
            else {
                i++;
            }
        }
        return new int[]{-1, -1};
    }
}
```

---

### Example

**Input**

```java
[2,7,11,15], target = 9
```

**Output**

```java
[1,2]
```

Subarray:

```java
2 + 7 = 9
```

---

### Complexity

- Time: **O(n)**
    
- Space: **O(1)**
    

---

### Interview Notes

Pattern:

```java
Two Pointer (Opposite Ends)
```

Key condition:

```java
Array must be sorted
```

---

### Brutal Truth

If you:

- Used **HashMap** → you missed the whole point
    
- Tried brute force → you’re wasting time
    
- Forgot it's **1-indexed** → easy reject
    

This is one of the **easiest medium problems**, but also one of the most common filters.

---

### Extra Insight

Why two pointers works:

- Sorted array gives **monotonic behavior**
    
- Moving pointers intelligently **guarantees progress**
    

---
## 2️⃣ Remove Duplicates from Sorted Array

🔗 LeetCode: [https://leetcode.com/problems/remove-duplicates-from-sorted-array/](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

---

### Description

Given a **sorted array** `nums`, remove duplicates **in-place** such that:

- Each unique element appears **only once**
    
- Maintain **relative order**
    
- Return number of unique elements `k`
    

Final expectation:

```java
First k elements → unique
Rest → irrelevant
```

---

### Core Insight

Sorted array → duplicates are **adjacent**

So:

```java
Two Pointer (Slow + Fast)
```

- `i` → last unique element position
    
- `j` → scans array
    

---

### Algorithm

- Initialize:
    
    - `i = 0` (points to last unique)
        
    - `j = 1` (scanner)
        
- Traverse:
    
    - If `nums[j] != nums[i]`:
        
        - Move `i`
            
        - Place new unique element at `i`
            

---

### Code (Java)

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int n = nums.length;
        if (n == 0) return 0;

        int i = 0;

        for (int j = 1; j < n; j++) {
            if (nums[j] != nums[i]) {
                i++;
                nums[i] = nums[j];
            }
        }

        return i + 1;
    }
}
```

---

### Example

**Input**

```java
[0,0,1,1,1,2,2,3,3,4]
```

**Output**

```java
5
```

Modified array:

```java
[0,1,2,3,4,_,_,_,_,_]
```

---

### Complexity

- Time: **O(n)**
    
- Space: **O(1)**
    

---

### Interview Notes

Pattern:

```java
Two Pointer (In-place overwrite)
```

Use when:

- Sorted array
    
- Remove / compress elements
    
- No extra space allowed
    

---

### Brutal Truth

Your code works — but you overcomplicated it.

Problems in your version:

- `ans` variable → redundant (you already have `i`)
    
- Comparing `nums[j] == nums[j-1]` → unnecessary dependency
    
- Extra branching → more chances to screw up in interviews
    

Cleaner logic:

```java
Compare with nums[i], not nums[j-1]
```

Why?

- `i` always tracks last valid unique
    
- Keeps logic consistent and scalable
    

---

### Extra Insight

Return value:

```java
i + 1
```

Not `i`

Because:

- `i` is index
    
- length = index + 1
    

---

## 3️⃣ Squares of a Sorted Array

🔗 LeetCode: [https://leetcode.com/problems/squares-of-a-sorted-array/](https://leetcode.com/problems/squares-of-a-sorted-array/)

---

### Description

Given an integer array `nums` sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

---

### Core Insight

Sorted array with negatives → after squaring, largest values are always at **one of the two ends**.

So fill the result array **from back to front** using two pointers.

---

### Algorithm

- Initialize:
    - `i = 0` (left end)
    - `j = n - 1` (right end)
    - `k = n - 1` (fill position)
- While `i <= j`:
    - Square both ends
    - Place the larger one at `res[k]`, move that pointer inward
    - `k--`

---

### Code (Java)

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int n = nums.length;
        int[] res = new int[n];
        int i = 0, j = n - 1, k = n - 1;

        while (i <= j) {
            int left = nums[i] * nums[i];
            int right = nums[j] * nums[j];

            if (left > right) {
                res[k--] = left;
                i++;
                //(means) 
                // res[k] = left;
                // k--;
                // i++
            } else {
                res[k--] = right;
                j--;
            }
        }
        return res;
    }
}
```

---

### Example

**Input:** `[-4,-1,0,3,10]` **Output:** `[0,1,9,16,100]`

---

### Complexity

- Time: **O(n)**
- Space: **O(n)** (result array)

---

### Interview Notes

Pattern:

```
Two Pointer (Opposite Ends) + Reverse Fill
```

Key condition:

```
Array is sorted — largest square is always at one of the two ends
```

---

### Brutal Truth

If you:

- Squared everything and sorted → O(n log n), misses the follow-up entirely
- Separated negatives and positives → works but overcomplicated, extra space
- Forgot `i <= j` (used `i < j`) → drops the middle element when array has odd length

---
## 4️⃣ 3Sum

🔗 LeetCode: [https://leetcode.com/problems/3sum/](https://leetcode.com/problems/3sum/)

---

### Description

Given an integer array `nums`, return all unique triplets `[nums[i], nums[j], nums[k]]` such that they sum to `0`. No duplicate triplets allowed.

---

### Core Insight

Fix one element → reduce to **Two Sum on sorted array**.

Sorting enables:

- Pointer movement based on sum comparison
- Duplicate skipping without a HashSet

---

### Algorithm

- Sort the array
- For each `i` from `0` to `n-2`:
    - Skip if duplicate (`nums[i] == nums[i-1]`)
    - Set `target = -nums[i]`
    - Two pointers: `j = i+1`, `k = n-1`
    - While `j < k`:
        - sum == target → add triplet, move both, skip duplicates
        - sum < target → `j++`
        - sum > target → `k--`

---

### Code (Java)

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        List<List<Integer>> ans = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i < n - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;

            int j = i + 1;
            int k = n - 1;
            int target = -nums[i];

            while (j < k) {
                int sum = nums[j] + nums[k];

                if (sum == target) {
                    ans.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    j++;
                    k--;
                    while (j < n && nums[j] == nums[j - 1]) j++;
                    while (k >= 0 && nums[k] == nums[k + 1]) k--;
                }
                else if (sum < target) j++;
                else k--;
            }
        }
        return ans;
    }
}
```

---

### Example

**Input:** `[-1,0,1,2,-1,-4]` **Output:** `[[-1,-1,2],[-1,0,1]]`

---

### Complexity

- Time: **O(n²)**
- Space: **O(1)** (excluding output)

---

### Interview Notes

Pattern:

```
Sort + Fix One + Two Pointers
```

Key condition:

```
Sorting is mandatory — enables both pointer movement and duplicate skipping
```

---

### Brutal Truth

If you:

- Skip duplicate check for `i` → duplicate triplets in output
- Skip duplicate skip after finding triplet → duplicate triplets again
- Use `i < n` in inner while guards instead of `j < n` and `k >= 0` → index out of bounds
- Use HashSet instead of sorting → works but misses the pattern entirely

---

## 5️⃣ 3Sum Closest

🔗 LeetCode: [https://leetcode.com/problems/3sum-closest/](https://leetcode.com/problems/3sum-closest/)

---

### Description

Given an integer array `nums` and a `target`, find three integers such that their sum is closest to `target`. Return that sum. Exactly one solution guaranteed.

---

### Core Insight

Same structure as 3Sum — fix one, two pointers on the rest.

Difference: instead of checking `== 0`, track **minimum absolute difference** from target.

---

### Algorithm

- Sort the array
- Initialize `cloSum = nums[0] + nums[1] + nums[2]`
- For each `i` from `0` to `n-2`:
    - `j = i+1`, `k = n-1`
    - While `j < k`:
        - Compute `currSum`
        - If `currSum == target` → return immediately
        - If `|currSum - target| < |cloSum - target|` → update `cloSum`
        - If `currSum < target` → `j++`
        - If `currSum > target` → `k--`
- Return `cloSum`

---

### Code (Java)

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int n = nums.length;
        Arrays.sort(nums);
        int cloSum = nums[0] + nums[1] + nums[2];

        for (int i = 0; i < n - 2; i++) {
            int j = i + 1;
            int k = n - 1;

            while (j < k) {
                int currSum = nums[i] + nums[j] + nums[k];

                if (currSum == target) return currSum;

                if (Math.abs(currSum - target) < Math.abs(cloSum - target)) {
                    cloSum = currSum;
                }
                else if (currSum < target) {
                    j++;
                }
                else {
                    k--;
                }
            }
        }
        return cloSum;
    }
}
```

---

### Example

**Input:** `[-1,2,1,-4], target = 1` **Output:** `2` **Triplet:** `-1 + 2 + 1 = 2`

---

### Complexity

- Time: **O(n²)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Sort + Fix One + Two Pointers (Closest variant)
```

Key condition:

```
Update closest BEFORE moving pointers — not inside the else branch
```

---

### Brutal Truth

If you:

- Put `cloSum` update inside `else if` or `else` → misses updates when difference is smaller but sum isn't in the right direction
- Initialize `cloSum` to `Integer.MAX_VALUE` → works but cleaner to init with first triplet
- Forget early return on `currSum == target` → wastes time, answer can't get better than 0 diff

---

## 6️⃣ Sort Colors

🔗 LeetCode: [https://leetcode.com/problems/sort-colors/](https://leetcode.com/problems/sort-colors/)

---

### Description

Given an array `nums` with `n` objects colored `0` (red), `1` (white), or `2` (blue), sort them in-place so same colors are adjacent in order `0 → 1 → 2`. No library sort allowed.

---

### Core Insight

Only 3 possible values → partition into **3 regions** in one pass.

```
Dutch National Flag Algorithm
```

Three pointers:

- `low` → boundary of 0s
- `mid` → current element being examined
- `high` → boundary of 2s

---

### Algorithm

- Initialize: `low = 0`, `mid = 0`, `high = n-1`
- While `mid <= high`:
    - `nums[mid] == 0` → swap with `low`, `low++`, `mid++`
    - `nums[mid] == 1` → `mid++`
    - `nums[mid] == 2` → swap with `high`, `high--` (do NOT move `mid`)

---

### Code (Java)

```java
class Solution {
    public void sortColors(int[] nums) {
        int n = nums.length;
        int low = 0, mid = 0, high = n - 1;

        while (mid <= high) {
            if (nums[mid] == 0) {
                swap(nums, low, mid);
                low++;
                mid++;
            }
            else if (nums[mid] == 1) {
                mid++;
            }
            else {
                swap(nums, mid, high);
                high--;
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

---

### Example

**Input:** `[2,0,2,1,1,0]` **Output:** `[0,0,1,1,2,2]`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Dutch National Flag (3-way partition)
```

Key condition:

```
Only 3 distinct values → 3 pointer regions in one pass
```

---

### Brutal Truth

If you:

- Move `mid` after swapping with `high` → unexamined element gets skipped, wrong output
- Use counting sort (count 0s, 1s, 2s, refill) → works but misses the follow-up entirely
- Use `mid < high` instead of `mid <= high` → last element never processed

---
## 7️⃣ 4Sum

🔗 LeetCode: [https://leetcode.com/problems/4sum/](https://leetcode.com/problems/4sum/)

---

### Description

Given an array `nums` and integer `target`, return all unique quadruplets `[a,b,c,d]` such that they sum to `target`. No duplicate quadruplets allowed.

---

### Core Insight

Same as 3Sum — fix two elements, reduce to **Two Pointers on the rest**.

Natural extension: 2Sum → 3Sum → 4Sum → kSum

---

### Algorithm

- Sort the array
- For each `i` from `0` to `n-3`:
    - Skip if duplicate
    - For each `j` from `i+1` to `n-2`:
        - Skip if duplicate
        - `k = j+1`, `l = n-1`
        - While `k < l`:
            - sum == target → add quadruplet, move both, skip duplicates
            - sum < target → `k++`
            - sum > target → `l--`

---

### Code (Java)

```java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        int n = nums.length;
        List<List<Integer>> ans = new ArrayList<>();

        if (n < 4) return ans;

        Arrays.sort(nums);

        for (int i = 0; i < n - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;

            for (int j = i + 1; j < n - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;

                int k = j + 1;
                int l = n - 1;

                while (k < l) {
                    long sum = (long) nums[i] + nums[j] + nums[k] + nums[l];

                    if (sum == target) {
                        ans.add(Arrays.asList(nums[i], nums[j], nums[k], nums[l]));
                        k++;
                        l--;
                        while (k < l && nums[k] == nums[k - 1]) k++;
                        while (k < l && nums[l] == nums[l + 1]) l--;
                    }
                    else if (sum < target) k++;
                    else l--;
                }
            }
        }
        return ans;
    }
}
```

---

### Example

**Input:** `[1,0,-1,0,-2,2], target = 0` **Output:** `[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]`

---

### Complexity

- Time: **O(n³)**
- Space: **O(1)** (excluding output)

---

### Interview Notes

Pattern:

```
Sort + Fix Two + Two Pointers
```

Key condition:

```
Duplicate skip at ALL levels — i, j, and after finding valid quadruplet
```

---

### Brutal Truth

If you:

- Use `int` instead of `long` for sum → silent overflow for large values, wrong answers
- Skip `n < 4` guard → `n-3` becomes negative index, crashes
- Duplicate skip for `j` uses `j > i` instead of `j > i+1` → skips valid `j` on first iteration of each `i`
- Miss inner duplicate skips after adding quadruplet → duplicate results in output

---
## 8️⃣ Backspace String Compare

🔗 LeetCode: [https://leetcode.com/problems/backspace-string-compare/](https://leetcode.com/problems/backspace-string-compare/)

---

### Description

Given two strings `s` and `t`, return `true` if they're equal when typed into empty text editors, where `#` means backspace.

---

### Core Insight

Process from the **right end** — a `#` only affects characters that come before it, so working backward lets you resolve backspaces without building the final string.

```
O(1) space by walking right-to-left with a skip counter
```

---

### Algorithm

- Two pointers `i = s.length-1`, `j = t.length-1`
- While `i >= 0` or `j >= 0`:
    - For `s`: skip characters using `skipS` counter
        - `#` → `skipS++`, move left
        - `skipS > 0` → consume one skip, move left
        - else → stop, this is a real character
    - Do the same for `t` with `skipT`
    - Compare current valid characters at `i` and `j`
        - mismatch → `false`
        - one ran out, other didn't → `false`
    - Move both pointers left

---

### Code (Java)

```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        int i = s.length() - 1;
        int j = t.length() - 1;

        int skipS = 0;
        int skipT = 0;

        while (i >= 0 || j >= 0) {
            while (i >= 0) {
                if (s.charAt(i) == '#') {
                    skipS++;
                    i--;
                }
                else if (skipS > 0) {
                    skipS--;
                    i--;
                }
                else {
                    break;
                }
            }

            while (j >= 0) {
                if (t.charAt(j) == '#') {
                    skipT++;
                    j--;
                }
                else if (skipT > 0) {
                    skipT--;
                    j--;
                }
                else {
                    break;
                }
            }

            if (i >= 0 && j >= 0 && s.charAt(i) != t.charAt(j)) {
                return false;
            }
            else if ((i >= 0) != (j >= 0)) {
                return false;
            }

            i--;
            j--;
        }
        return true;
    }
}
```

---

### Example

**Input:** `s = "ab#c", t = "ad#c"` **Output:** `true` **Both become:** `"ac"`

---

### Complexity

- Time: **O(n + m)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Two Pointer (Right to Left) + Skip Counter
```

Key condition:

```
Process from the end — backspaces only ever cancel what's already behind them
```

---

### Brutal Truth

If you:

- Use a stack to build the actual string → works, O(n) space, fails the follow-up
- Forget the `(i >= 0) != (j >= 0)` check → misses the case where one string still has valid chars and the other doesn't
- Try to process left-to-right → backspace effect isn't known until you see the `#`, forces a second pass or stack anyway

---

**New pattern alert:** this doesn't fit your existing Pattern 1 (Opposite Ends) or Pattern 2 (Slow+Fast). It's a distinct sub-pattern — **Two Pointer from the Right with a state counter**. Worth its own slot under Two Pointers:

```
Pattern 3 — Right-to-Left with Skip State
Use when: undo/cancel operations (backspace, bracket matching from end), need to resolve state before comparing
Core: walk backward, use a counter to track pending "cancellations", only compare real characters
```

---
## 9️⃣ Shortest Unsorted Continuous Subarray

🔗 LeetCode: [https://leetcode.com/problems/shortest-unsorted-continuous-subarray/](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/)

---

### Description

Given an integer array `nums`, find the shortest continuous subarray such that if only that subarray is sorted, the entire array becomes sorted.

Return the length of that subarray.

---

### Core Insight

A sorted array has:

```text
nums[i] <= nums[i+1]
```

everywhere.

Find:

- First position from the left where order breaks → `low`
    
- First position from the right where order breaks → `high`
    

The segment `[low, high]` is unsorted, but some elements outside it may also belong inside after sorting.

So:

1. Find `min` and `max` inside `[low, high]`
    
2. Expand left while previous elements are larger than `min`
    
3. Expand right while next elements are smaller than `max`
    

---

### Algorithm

- Find first inversion from left:
    

```java
nums[i] > nums[i+1]
```

→ `low`

- If none found, array is already sorted → return `0`
    
- Find first inversion from right:
    

```java
nums[i-1] > nums[i]
```

→ `high`

- Find:
    

```java
wMin = minimum element in [low,high]
wMax = maximum element in [low,high]
```

- Expand:
    

```java
while(low > 0 && nums[low-1] > wMin)
    low--;

while(high < n-1 && nums[high+1] < wMax)
    high++;
```

Return:

```java
high - low + 1
```

---

### Code (Java)

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int n = nums.length;
        int low = 0, high = n - 1;

        while (low < n - 1 && nums[low] <= nums[low + 1])
            low++;

        if (low == n - 1)
            return 0;

        while (high > 0 && nums[high - 1] <= nums[high])
            high--;

        int wMin = Integer.MAX_VALUE;
        int wMax = Integer.MIN_VALUE;

        for (int i = low; i <= high; i++) {
            wMin = Math.min(wMin, nums[i]);
            wMax = Math.max(wMax, nums[i]);
        }

        while (low > 0 && nums[low - 1] > wMin)
            low--;

        while (high < n - 1 && nums[high + 1] < wMax)
            high++;

        return high - low + 1;
    }
}
```

---

### Example

**Input**

```java
[2,6,4,8,10,9,15]
```

Initial inversions:

```text
low = 1 (6 > 4)
high = 5 (10 > 9)
```

Window:

```text
[6,4,8,10,9]
```

```text
wMin = 4
wMax = 10
```

No further expansion needed.

Output:

```java
5
```

---

### Complexity

- Time: **O(n)**
    
- Space: **O(1)**
    

---

### Interview Notes

Pattern:

```text
Two Pointer (Opposite Ends) + Boundary Expansion
```

Key condition:

```text
Array is almost sorted.
Find where order first breaks from both sides.
Expand using min and max of the broken window.
```

---

### Brutal Truth

If you:

- Compute `min` and `max` over the whole array → wrong answer.
    
- Forget the already sorted case → return negative length.
    
- Expand before finding `wMin` and `wMax` → miss elements that belong in the window.
    
- Sort the whole array and compare → O(n log n), misses the follow-up.
    

---

### Extra Insight

This is not a sliding window problem.

Think of it as:

```text
Opposite-End Two Pointers
        +
Find Broken Region
        +
Boundary Expansion using min/max
```

Pattern:

```text
Pattern 4 — Boundary Expansion

Use when:
- Array is almost sorted
- Need smallest segment to fix the entire array

Core:
Find first disorder from both ends, then expand using min and max inside that disorder.
```