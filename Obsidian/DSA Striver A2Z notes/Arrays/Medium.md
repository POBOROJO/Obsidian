---
tags:
  - arrays
  - hard
---
## ✅ Problem Tracker - 
- [x] 6️⃣ Two Sum 
- [x] 7️⃣ Sort Colors 
- [x] 8️⃣ Majority Element 
- [x] 9️⃣ Maximum Subarray 
- [x] 🔟 Best Time to Buy and Sell Stock 
- [x] 1️⃣1️⃣ Rearrange Array Elements by Sign 
- [x] 1️⃣2️⃣ Next Permutation 
- [x] 1️⃣3️⃣ Array Leaders 
- [x] 1️⃣4️⃣ Longest Consecutive Sequence 
- [x] 1️⃣5️⃣ Rotate Image 
- [x] 1️⃣6️⃣ Spiral Matrix 

## 6️⃣ Two Sum

### Core Insight

For each number, check if its **complement** already exists using a hash map.

---

### Algorithm

- Store `value → index`
    
- On each step, check `target - nums[i]`
    

---

### Code (Java)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int need = target - nums[i];
            if (map.containsKey(need)) {
                return new int[]{map.get(need), i};
            }
            map.put(nums[i], i);
        }
        return new int[]{};
    }
}
```

---

### Example

**Input:** `[2,7,11,15], target = 9`  
**Output:** `[0,1]`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(n)**
    

---

### Interview Notes

- One-pass is optimal
    
- Handles duplicates
    
- Pattern: **Hashing / Complement Lookup**
    

---

## 7️⃣ Sort Colors (Dutch National Flag)

### Core Insight

Only `0,1,2` → partition array into **three regions** in one pass.

---

### Algorithm

Pointers:

- `low` → 0s
    
- `mid` → current
    
- `high` → 2s
    

Rules:

- `0` → swap with `low`
    
- `1` → move `mid`
    
- `2` → swap with `high` (don’t move `mid`)
    

---

### Code (Java)

```java
class Solution {
    public void sortColors(int[] nums) {
        int low = 0, mid = 0, high = nums.length - 1;

        while (mid <= high) {
            if (nums[mid] == 0) {
                int t = nums[low];
                nums[low] = nums[mid];
                nums[mid] = t;
                low++; mid++;
            } else if (nums[mid] == 1) {
                mid++;
            } else {
                int t = nums[high];
                nums[high] = nums[mid];
                nums[mid] = t;
                high--;
            }
        }
    }
}
```

---

### Example

**Input:** `[2,0,2,1,1,0]`  
**Output:** `[0,0,1,1,2,2]`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(1)**
    

---

### Interview Notes

- Never increment `mid` after swapping with `high`
    
- Counting sort is inferior here
    
- Pattern: **Dutch National Flag**
    

---

## 8️⃣ Majority Element (Boyer–Moore Voting)

### Core Insight

If an element appears **more than n/2 times**, it will **survive all pair cancellations**.  
Boyer–Moore exploits this by canceling different elements against each other.

---

### Algorithm

**Phase 1: Find candidate**

- Maintain `count` and `candidate`
    
- If `count == 0` → pick current element as candidate
    
- Same element → `count++`
    
- Different element → `count--`
    

**Phase 2: Verify candidate**

- Count occurrences of candidate
    
- If count > ⌊n/2⌋ → answer
    

> (Verification is optional here because the problem guarantees existence, but safe to include.)

---

### Code (Java)

```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        int el = 0;

        // Phase 1: find candidate
        for (int num : nums) {
            if (count == 0) {
                el = num;
                count = 1;
            } else if (num == el) {
                count++;
            } else {
                count--;
            }
        }

        // Phase 2: verify (optional here)
        int freq = 0;
        for (int num : nums) {
            if (num == el) freq++;
        }

        return freq > nums.length / 2 ? el : -1;
    }
}
```

---

### Example

**Input:** `[2,2,1,1,1,2,2]`  
**Output:** `2`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(1)**
    

---

### Interview Notes

- Works **only** when majority element (> n/2) exists
    
- No sorting, no hashing, constant space
    
- Second pass can be skipped if majority existence is guaranteed
    
- Pattern: **Boyer–Moore Voting Algorithm**
    

---

## 9️⃣ Maximum Subarray (Kadane’s Algorithm) (max sub array ka sum)

### Core Insight

A subarray with negative sum is **never worth extending**.  
Drop it and start fresh — only positive prefixes help future sums.

---

### Algorithm

- Initialize:
    
    - `sum = 0` → current subarray sum
        
    - `maxi = -∞` → best answer so far
        
- Traverse the array:
    
    - Add current element to `sum`
        
    - Update `maxi = max(maxi, sum)`
        
    - If `sum < 0`, reset `sum = 0`
        
- Return `maxi`
    

---

### Code (Java)

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxi = Integer.MIN_VALUE;
        int sum = 0;

        for (int x : nums) {
            sum += x;
            maxi = Math.max(maxi, sum);
            if (sum < 0) sum = 0;
        }
        return maxi;
    }
}
```

---

### Example

**Input:** `[-2,1,-3,4,-1,2,1,-5,4]`  
**Output:** `6`  
**Subarray:** `[4,-1,2,1]`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(1)**
    

---

### Interview Notes

- Handles **all-negative arrays** correctly because `maxi` starts at `-∞`
    
- Reset happens **after** updating `maxi` (critical)
    
- This is **Kadane’s Algorithm** — mandatory to know
    
- Pattern: **Prefix Sum / Greedy Optimization**
    

---

## 🔟 Best Time to Buy and Sell Stock

### Core Insight

Maximum profit = **best future sell price − lowest price seen so far**.  
You never need to track days explicitly — just keep the **minimum prefix**.

---

### Algorithm

- Initialize:
    
    - `mini` = price on day 0 (best buy so far)
        
    - `maxProfit` = 0
        
- Traverse prices from day 1 onward:
    
    - Profit if sold today = `prices[i] − mini`
        
    - Update `maxProfit`
        
    - Update `mini` if current price is lower
        
- Return `maxProfit`
    

---

### Code (Java)

```java
class Solution {
    public int maxProfit(int[] prices) {
        int mini = prices[0];
        int maxProfit = 0;

        for (int i = 1; i < prices.length; i++) {
            int profit = prices[i] - mini;
            maxProfit = Math.max(maxProfit, profit);
            mini = Math.min(mini, prices[i]);
        }

        return maxProfit;
    }
}
```

---

### Example

**Input:** `[7,1,5,3,6,4]`  
**Output:** `5`  
**Buy:** `1`, **Sell:** `6`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(1)**
    

---

### Interview Notes

- Buy must happen **before** sell → prefix minimum enforces this
    
- Works even when prices always decrease → returns `0`
    
- Do **not** confuse with multiple-transaction stock problems
    
- Pattern: **Prefix Minimum / Greedy Scan**
    

---

## 1️⃣1️⃣ Rearrange Array Elements by Sign

### Core Insight

Positives and negatives are **equal in count** and order must be preserved.  
Place positives at **even indices** and negatives at **odd indices**.

No swaps. No sorting. Just **direct placement**.

---

### Algorithm

- Create a result array `ans` of same size
    
- Use two pointers:
    
    - `pos = 0` → next position for positive numbers
        
    - `neg = 1` → next position for negative numbers
        
- Traverse original array:
    
    - If number is positive → place at `ans[pos]`, `pos += 2`
        
    - If number is negative → place at `ans[neg]`, `neg += 2`
        
- Return `ans`
    

---

### Code (Java)

```java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        int pos = 0, neg = 1;

        for (int x : nums) {
            if (x < 0) {
                ans[neg] = x;
                neg += 2;
            } else {
                ans[pos] = x;
                pos += 2;
            }
        }
        return ans;
    }
}
```

---

### Example

**Input:** `[3,1,-2,-5,2,-4]`  
**Output:** `[3,-2,1,-5,2,-4]`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(n)** (extra array required)
    

---

### Interview Notes

- Order preservation makes **in-place swapping impossible**
    
- Starting with positive → even indices
    
- Equal count of positives and negatives is a **critical guarantee**
    
- Pattern: **Index Mapping / Stable Partition**
    

---

## 1️⃣2️⃣ Next Permutation

### Core Insight

The next lexicographical permutation is obtained by:

- **Breaking the longest non-increasing suffix**
    
- Making the **smallest possible increase**
    
- Resetting the suffix to the **lowest order**
    

This guarantees the _next_ permutation, not just any bigger one.

---

### Algorithm

1. Scan from right to left to find the first index `index` such that  
    `nums[index] < nums[index + 1]`  
    → this is the **pivot**
    
2. If no such index exists:
    
    - Array is the **last permutation**
        
    - Reverse the whole array and return
        
3. From the right, find the **smallest element greater than pivot**
    
4. Swap it with the pivot
    
5. Reverse the subarray to the right of the pivot
    

---

### Code (Java)

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;
        int index = -1;

        // 1. Find pivot
        for (int i = n - 2; i >= 0; i--) {
            if (nums[i] < nums[i + 1]) {
                index = i;
                break;
            }
        }

        // 2. If no pivot, reverse entire array
        if (index == -1) {
            reverse(nums, 0, n - 1);
            return;
        }

        // 3. Find rightmost successor
        for (int i = n - 1; i > index; i--) {
            if (nums[i] > nums[index]) {
                swap(nums, i, index);
                break;
            }
        }

        // 4. Reverse suffix
        reverse(nums, index + 1, n - 1);
    }

    private void reverse(int[] arr, int l, int r) {
        while (l < r) {
            swap(arr, l++, r--);
        }
    }

    private void swap(int[] arr, int i, int j) {
        int t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }
}
```

---

### Example

**Input:** `[1,2,3]`  
**Output:** `[1,3,2]`

**Input:** `[3,2,1]`  
**Output:** `[1,2,3]`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(1)**
    

---

### Interview Notes

- Reversing the suffix is **mandatory**, not sorting
    
- Pivot search must go **right → left**
    
- Handles duplicates correctly
    
- Pattern: **Lexicographical Permutation / Greedy**
    
---
## 1️⃣3️⃣ Array Leaders

### Core Insight

An element is a **leader** if it is **≥ every element to its right**.  
Scanning from the **right** lets you track the maximum seen so far in one pass.

---

### Algorithm

- Initialize `maxi = -∞`
    
- Traverse array from **right to left**
    
- If `arr[i] ≥ maxi`:
    
    - Update `maxi`
        
    - Add element to result
        
- Reverse the result (because leaders were collected right → left)
    

---

### Code (Java)

```java
class Solution {
    static ArrayList<Integer> leaders(int arr[]) {
        ArrayList<Integer> ans = new ArrayList<>();
        int n = arr.length;
        int maxi = Integer.MIN_VALUE;

        for (int i = n - 1; i >= 0; i--) {
            if (arr[i] >= maxi) {
                maxi = arr[i];
                ans.add(maxi);
            }
        }

        Collections.reverse(ans);
        return ans;
    }
}
```

---

### Example

**Input:** `[16, 17, 4, 3, 5, 2]`  
**Output:** `[17, 5, 2]`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(k)** where `k` = number of leaders (output space)
    

---

### Interview Notes

- Rightmost element is **always a leader**
    
- Use `≥`, not `>` (duplicates allowed)
    
- Left-to-right scan leads to **O(n²)** → reject
    
- Pattern: **Suffix Maximum / Right-to-Left Scan**
    

---

## 1️⃣4️⃣ Longest Consecutive Sequence

### Core Insight

Only start counting a sequence from a number **that has no predecessor** (`x-1`).  
This avoids re-counting and guarantees **O(n)** time.

---

### Algorithm

- Insert all numbers into a `HashSet`
    
- For each number `x` in the set:
    
    - If `x-1` does **not** exist → `x` is the start of a sequence
        
    - Count consecutive numbers `x, x+1, x+2...`
        
    - Update longest length
        
- Return the maximum length found
    

---

### Code (Java)

```java
class Solution {
    public int longestConsecutive(int[] nums) {
         int n = nums.length;

        // If the array is empty, no sequence exists
        if (n == 0) return 0;

        // Variable to store the longest sequence length found
        int longest = 1; 

        // HashSet to store unique elements for O(1) lookup
        Set<Integer> st = new HashSet<>();

        // Add all elements to the set to remove duplicates
        for (int i = 0; i < n; i++) {
            st.add(nums[i]);
        }

        /* Loop through each element in the set to find 
           the starting point of consecutive sequences */
        for (int it : st) {
            // If there is no number before 'it', it’s the start of a sequence
            if (!st.contains(it - 1)) {
                // Start the count for this sequence
                int cnt = 1; 
                // Store the current number
                int x = it; 

                // Keep checking for the next consecutive number
                while (st.contains(x + 1)) {
                    // Move to the next number in sequence
                    x = x + 1; 
                    // Increment the length of current sequence
                    cnt = cnt + 1; 
                }

                // Update the longest sequence length if needed
                longest = Math.max(longest, cnt);
            }
        }

        // Return the length of the longest sequence
        return longest;
    }
}
```

---

### Example

**Input:** `[100,4,200,1,3,2]`  
**Output:** `4`  
**Sequence:** `[1,2,3,4]`

---

### Complexity

- Time: **O(n)**  
    (Each number processed at most twice)
    
- Space: **O(n)**
    

---

### Interview Notes

- Sorting gives **O(n log n)** → rejected by constraint
    
- Checking only sequence starts is the **key optimization**
    
- Duplicates are naturally handled by `HashSet`
    
- Pattern: **Hashing + Sequence Detection**
    

---

## 1️⃣5️⃣ Rotate Image (90° Clockwise)

### Core Insight

A 90° clockwise rotation =  
**Transpose + Reverse each row**

This converts rows → columns, then fixes orientation.

---

### Algorithm

1. **Transpose matrix**
    
    - Swap `matrix[i][j]` with `matrix[j][i]` for `j > i`
        
2. **Reverse each row**
    
    - Two-pointer swap from ends
        

Done in-place.

---

### Code (Java)

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        // 1. Transpose
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int t = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = t;
            }
        }

        // 2. Reverse each row
        for (int i = 0; i < n; i++) {
            int l = 0, r = n - 1;
            while (l < r) {
                int t = matrix[i][l];
                matrix[i][l] = matrix[i][r];
                matrix[i][r] = t;
                l++; r--;
            }
        }
    }
}
```

---

### Example
#### Input Matrix:
$$
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}
$$

#### Output Matrix:
$$
\begin{bmatrix}
7 & 4 & 1 \\
8 & 5 & 2 \\
9 & 6 & 3
\end{bmatrix}
$$

This transformation appears to be a 90-degree counterclockwise rotation of the original matrix. The first column of the input becomes the last row of the output, the second column becomes the middle row, and so on.

---
### Complexity

- Time: **O(n²)**
    
- Space: **O(1)**
---

### Interview Notes

- Must be **in-place**
    
- Transpose loop uses `j = i+1` to avoid double swaps
    
- Clockwise = transpose + row-reverse
    
- Anti-clockwise = transpose + column-reverse
    
- Pattern: **Matrix Transformation**

---

## 1️⃣6️⃣ Spiral Matrix

### Core Insight

Traverse the matrix layer by layer using **4 boundaries**.  
Shrink boundaries after completing each direction.

---

### Algorithm

Maintain:

- `top`, `bottom`
    
- `left`, `right`
    

Repeat while valid:

1. Traverse **left → right** on `top`, then `top++`
    
2. Traverse **top → bottom** on `right`, then `right--`
    
3. If rows remain: traverse **right → left** on `bottom`, then `bottom--`
    
4. If columns remain: traverse **bottom → top** on `left`, then `left++`
    

---

### Code (Java)

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<>();

        int n = matrix.length, m = matrix[0].length;
        int top = 0, bottom = n - 1;
        int left = 0, right = m - 1;

        while (top <= bottom && left <= right) {

            for (int i = left; i <= right; i++)
                result.add(matrix[top][i]);
            top++;

            for (int i = top; i <= bottom; i++)
                result.add(matrix[i][right]);
            right--;

            if (top <= bottom) {
                for (int i = right; i >= left; i--)
                    result.add(matrix[bottom][i]);
                bottom--;
            }

            if (left <= right) {
                for (int i = bottom; i >= top; i--)
                    result.add(matrix[i][left]);
                left++;
            }
        }

        return result;
    }
}
```

---

### Example

**Input:**  
$$
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}
$$

**Output:** 
$$
\begin{bmatrix}
1 & 2 & 3 & 6 & 9 & 8 & 7 & 4 & 5
\end{bmatrix}
$$

---

### Complexity

- Time: **O(m × n)**
    
- Space: **O(1)** (excluding output)
    

---

### Interview Notes

- Boundary checks prevent duplicates in single row/column cases
    
- Each element visited exactly once
    
- Works for non-square matrices
    
- Pattern: **Matrix Boundary Traversal**

---
