---
tags:
  - arrays
  - hard
---
## ✅ Problem Tracker

- [x] 1️⃣8️⃣ Pascal's Triangle
- [x] 1️⃣9️⃣ Majority Element II (N/3 times)
- [x] 2️⃣0️⃣ 3Sum
- [x] 2️⃣1️⃣ 4Sum
- [x] 2️⃣2️⃣ Largest Subarray with 0 Sum
- [x] 2️⃣3️⃣ Count Subarrays with XOR = K
- [x] 2️⃣4️⃣ Merge Overlapping Intervals
- [x] 2️⃣5️⃣ Merge Two Sorted Arrays Without Extra Space
- [x] 2️⃣6️⃣ Merge Sorted Array
- [ ] 2️⃣7️⃣ Find Missing and Repeating Number
- [ ] 2️⃣8️⃣ Count Inversions (Merge Sort)
- [ ] 2️⃣9️⃣ Reverse Pairs (Merge Sort)
- [ ] 3️⃣0️⃣ Maximum Product Subarray

---

# 1️⃣8️⃣ Pascal's Triangle

🔗 [LeetCode 118](https://leetcode.com/problems/pascals-triangle/)

### Problem

Given an integer `numRows`, return the first `numRows` of Pascal's Triangle. Each number is the sum of the two numbers directly above it.

---

### Core Insight

Each row follows **nCr (binomial coefficients)**. Next value can be computed from previous using:

$$C = C \times \frac{(row - col)}{col}$$

Avoids recomputing sums and stays efficient.

---

### Algorithm

For each row `r` from `1 → numRows`:

- Start with `1`
- Use formula to generate next elements
- Add row to result

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

Input: `5` Output: `[[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]`

---

### Complexity

- Time: **O(n²)**
- Space: **O(n²)** (output storage)

---

### Interview Notes

- Uses combinatorics instead of DP
- `long` prevents overflow
- First and last elements are always `1`
- Pattern: **Combinatorics / Binomial Coefficients**

---

# 1️⃣9️⃣ Majority Element II (n/3 Version)

🔗 [LeetCode 229](https://leetcode.com/problems/majority-element-ii/)

### Problem

Given an array of size `n`, return all elements that appear more than ⌊n/3⌋ times.

Follow-up: Solve in **O(n)** time and **O(1)** space.

---

### Core Insight

More than ⌊n/3⌋ ⇒ **at most 2 such elements** can exist. So track **two candidates** and cancel others.

> If 3 numbers each appeared > n/3 → total > n → impossible.

---

### Algorithm

Phase 1: Find candidates

- Maintain `(el1,cnt1)` and `(el2,cnt2)`
- If number matches candidate → increment
- If count becomes 0 → assign new candidate
- Otherwise decrement both

Phase 2: Verify

- Count occurrences
- Add elements > n/3

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

Input: `[1,2,1,2,1,3]` Output: `[1]`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

- Extension of Boyer–Moore
- Maximum 2 valid answers
- Verification pass required
- Pattern: **Extended Boyer–Moore Voting**

---

# 2️⃣0️⃣ 3Sum

🔗 [LeetCode 15](https://leetcode.com/problems/3sum/)

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

- Duplicate skipping
- Efficient pointer movement

---

### Algorithm

1. Sort the array
2. For each index `i`:
    - Skip duplicates
    - Use two pointers `j = i+1`, `k = n-1`
3. While `j < k`:
    - If sum < 0 → `j++`
    - If sum > 0 → `k--`
    - If sum == 0:
        - Add triplet
        - Move both
        - Skip duplicates

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

Input: `[-1,0,1,2,-1,-4]` Output: `[[-1,-1,2],[-1,0,1]]`

---

### Complexity

- Time: **O(n²)**
- Space: **O(1)** (excluding output)

---

### Interview Notes

- Sorting mandatory
- Skip duplicates at all levels
- Optimal complexity is O(n²)
- Pattern: **Sorting + Two Pointers**

---

# 2️⃣1️⃣ 4Sum

🔗 [LeetCode 18](https://leetcode.com/problems/4sum/)

### Problem

Given array `nums` and integer `target`, return all unique quadruplets `[a,b,c,d]` such that:

```
a + b + c + d = target
```

---

### Core Insight

Fix two elements → reduce to **Two Pointer search**. Extension of 3Sum.

---

### Algorithm

1. Sort array
2. Fix `i`
    - Skip duplicates
3. Fix `j`
    - Skip duplicates
4. Use two pointers:
    - `k = j+1`
    - `l = n-1`
5. Compare sum:
    - < target → `k++`
        
    - > target → `l--`
        
    - == target → add & skip duplicates
        

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

Input: `[1,0,-1,0,-2,2]`, `target = 0` Output: `[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]`

---

### Complexity

- Time: **O(n³)**
- Space: **O(1)** (excluding output)

---

### Interview Notes

- Use `long` to avoid overflow
- Skip duplicates at all levels
- Natural extension: 2Sum → 3Sum → 4Sum → kSum
- Pattern: **Sorting + Nested Two Pointers**

---

# 2️⃣2️⃣ Largest Subarray with 0 Sum

🔗 [GFG - Largest Subarray with 0 Sum](https://www.geeksforgeeks.org/problems/largest-subarray-with-0-sum/1)

### Problem

Given an array containing positive and negative integers, find the **length of the longest contiguous subarray** whose sum is equal to `0`.

---

### Core Insight

If two prefix sums are equal at indices `i` and `j`, then the subarray between them has **sum = 0**.

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
            } else if (map.containsKey(sum)) {
                maxi = Math.max(maxi, i - map.get(sum));
            } else {
                map.put(sum, i);
            }
        }
        return maxi;
    }
}
```

---

### Example

**Input:** `[15, -2, 2, -8, 1, 7, 10, 23]` **Output:** `5` **Subarray:** `[-2, 2, -8, 1, 7]`

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

# 2️⃣3️⃣ Count Subarrays with Given XOR

🔗 [GFG - Count Subarrays with Given XOR](https://www.geeksforgeeks.org/problems/count-subarray-with-given-xor/1)

### Description

Given an array of integers `arr[]` and an integer `k`, return the **number of subarrays** whose XOR of all elements equals `k`.

A subarray must be contiguous.

---

### Core Insight

If:

```
prefixXor[j] ^ prefixXor[i-1] = k
```

Then:

```
prefixXor[i-1] = prefixXor[j] ^ k
```

So for every current prefix XOR, check how many times  
`currXor ^ k` has appeared before.

This is the **XOR version of Subarray Sum Equals K**.

---

### Algorithm

- Maintain:
    
    - `curr_xor`
        
    - `HashMap<prefixXor, frequency>`
        
    - `count`
        
- Initialize:
    
    - `mpp.put(0,1)` ==(important)==
- Traverse array:
    
    - `curr_xor ^= element`
        
    - Compute `remove = curr_xor ^ k`
        
    - Add `frequency(remove)` to `count`
        
    - Update frequency of `curr_xor`
        
- Return `count`
    

---

### Code (Java)

```java
class Solution {
    public long subarrayXor(int arr[], int k) {

        HashMap<Integer, Integer> mpp = new HashMap<>();
        int curr_xor = 0;
        long count = 0;

        mpp.put(0, 1); // base case

        for (int x : arr) {
            curr_xor ^= x;

            int remove = curr_xor ^ k;

            count += mpp.getOrDefault(remove, 0); // count++ => if remove is there in the mpp 

            mpp.put(curr_xor, mpp.getOrDefault(curr_xor, 0) + 1); // This line is mpp[curr_xor]++.
        }

        return count;
    }
}
```

---

### Example

**Input:**  
`arr = [4,2,2,6,4], k = 6`

**Output:**  
`4`

Subarrays:

- `[4,2]`
    
- `[4,2,2,6,4]`
    
- `[2,2,6]`
    
- `[6]`
    

---

### Complexity

- Time: **O(n)**
    
- Space: **O(n)**
    

---

### Interview Notes

- Initialize `mpp.put(0,1)` → critical
    
- Works because XOR has this property:
    
    ```
    A ^ B ^ B = A
    ```
    
- Sliding window does NOT work (XOR is not monotonic)
    
- Pattern: **Prefix XOR + HashMap**
    

---

## 2️⃣4️⃣ Merge Intervals

🔗 [LeetCode 56](https://leetcode.com/problems/merge-intervals/)

### Description

Given an array of intervals `intervals[i] = [start, end]`, merge all **overlapping intervals** and return the resulting non-overlapping intervals.

Two intervals overlap if:

```
start2 ≤ end1
```

The merged interval becomes:

```
[min(start1,start2), max(end1,end2)]
```

---

### Core Insight

If intervals are **sorted by start time**, overlapping intervals will appear **next to each other**.

So:

- Sort intervals
    
- Merge greedily with the last interval in the answer
    

---

### Algorithm

1. Sort intervals by `start`
    
2. Iterate through intervals
    
3. If:
    
    - result empty OR
        
    - current start > last merged end  
        → add new interval
        
4. Otherwise:
    
    - merge by updating end  
        `lastEnd = max(lastEnd, currentEnd)`

---

### Code (Java)

```java
class Solution {
    public int[][] merge(int[][] intervals) {

        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        List<int[]> ans = new ArrayList<>();

        for (int[] current : intervals) {

            if (ans.isEmpty() || current[0] > ans.get(ans.size() - 1)[1]) {
                ans.add(current);
            } 
            else {
                int[] last = ans.get(ans.size() - 1);
                last[1] = Math.max(last[1], current[1]);
            }
        }

        return ans.toArray(new int[ans.size()][]);
    }
}
```

---

### Example

**Input**

```
[[1,3],[2,6],[8,10],[15,18]]
```

**Output**

```
[[1,6],[8,10],[15,18]]
```

Explanation:

```
[1,3] overlaps [2,6] → merge → [1,6]
```

---

### Complexity

- Time: **O(n log n)** (sorting dominates)
    
- Space: **O(n)** (result storage)
    

---

### Interview Notes

- Sorting by start is the **key step**
    
- Overlap condition:
    

```
current.start ≤ last.end
```

- Adjacent intervals `[1,4]` and `[4,5]` **do merge**
    
- Pattern: **Greedy + Sorting**
     
---

## 2️⃣5️⃣ Merge Without Extra Space

🔗 [GFG - Merge Without Extra Space](https://www.geeksforgeeks.org/problems/merge-two-sorted-arrays-without-extra-space/1)

### Description

Given two **sorted arrays** `a[]` and `b[]` of sizes `n` and `m`, merge them into sorted order **without using extra space**.

After merging:

- `a[]` should contain the **first n elements**
    
- `b[]` should contain the **last m elements**
    

---

### Core Insight

The largest elements of `a` might belong in `b`.

So compare:

```
a[n-1] with b[0]
```

If `a[n-1] > b[0]`, swap them.

This pushes larger elements to the second array.

Then re-sort both arrays.

---

### Algorithm

1. Set:

```
left = n-1
right = 0
```

2. While `left >= 0` and `right < m`:

- If `a[left] > b[right]`
    
    → swap them
    
- Move pointers
    

3. Sort both arrays.

---

### Code (Java)

```java
class Solution {

    static void swap(int [] a, int i, int [] b, int j){
        int temp = a[i];
        a[i] = b[j];
        b[j] = temp;
    }

    public void mergeArrays(int a[], int b[]) {

        int n = a.length;
        int m = b.length;

        int left = n - 1;
        int right = 0;

        while (left >= 0 && right < m) {

            if (a[left] > b[right]) {
                swap(a, left, b, right);
                left--;
                right++;
            } 
            else {
                break;
            }
        }

        Arrays.sort(a);
        Arrays.sort(b);
    }
}
```

---

### Example

**Input**

```
a = [1,5,9,10,15,20]
b = [2,3,8,13]
```

**Output**

```
a = [1,2,3,5,8,9]
b = [10,13,15,20]
```

---

### Complexity

- Time: **O((n+m) log(n+m))** (because of sorting)
    
- Space: **O(1)**
    

---

### Interview Notes

Pattern 1:

```
Two Pointer Boundary Swap
```

Pattern 2:

```
In-place Merge (Gap Method is the optimal version)
```

Important follow-up:

Interviewers often expect the **Shell Sort Gap Method** for **O((n+m) log(n+m)) without sorting individually**.

---

# 2️⃣6️⃣ Merge Sorted Array

🔗 [LeetCode 88](https://leetcode.com/problems/merge-sorted-array/)

### Description

You are given two sorted arrays:

```
nums1 (size m+n)
nums2 (size n)
```

The first `m` elements of `nums1` are valid, and the last `n` are placeholders (`0`).

Merge `nums2` into `nums1` **in-place** so that `nums1` becomes fully sorted.

After merging:

- `a[]` should contain the **first n elements**
    
- `b[]` should contain the **last m elements**
    
Same idea but `nums1` has extra space at the back to hold the merged result. Merge **from the back** to avoid overwriting elements.

**Three pointers:**

```
i = m-1      (end of valid nums1)
j = n-1      (end of nums2)
k = m+n-1    (end of nums1 total)
```


---

### Core Insight

The largest elements of `a` might belong in `b`.

So compare:

```
a[n-1] with b[0]
```

If `a[n-1] > b[0]`, swap them.

This pushes larger elements to the second array.

Then re-sort both arrays.

---

### Algorithm

Set three pointers:

```
i = m-1
j = n-1
k = m+n-1
```

Compare from the end:

- Place the larger element at `nums1[k]`
    
- Move pointers accordingly
    
- If `nums2` still has elements, copy them
    

---

### Code (Java)

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {

        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;

        while (i >= 0 && j >= 0) {

            if (nums1[i] > nums2[j]) {
                nums1[k] = nums1[i];
                i--;
            } 
            else {
                nums1[k] = nums2[j];
                j--;
            }

            k--;
        }

        while (j >= 0) {
            nums1[k] = nums2[j];
            j--;
            k--;
        }
    }
}
```

---

### Example

**Input**

```
nums1 = [1,2,3,0,0,0]
nums2 = [2,5,6]
```

**Output**

```
[1,2,2,3,5,6]
```

---

### Complexity

- Time: **O(m+n)**
    
- Space: **O(1)**
    

---

### Interview Notes

Pattern 1:

```
Two Pointer Merge
```

Pattern 2:

```
Reverse Fill Strategy
```

This is the same idea used in:

- **Merge step of Merge Sort**
    
- **K-way array merging**
    
- **External sorting**
    

---
