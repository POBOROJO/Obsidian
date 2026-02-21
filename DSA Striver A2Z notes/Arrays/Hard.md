--- tags: [arrays, hard] --- 
## ✅ Problem Tracker - 
- [x] 1️⃣7️⃣ Pascal's Triangle  
- [x] 1️⃣8️⃣ Majority Element II
- [ ] 3Sum
- [ ] 4Sum
- [ ] Merge Overlapping Subintervals 
- [ ] Merge two sorted arrays without extra space
- [ ] Reverse Pairs
- [ ] Maximum Product Subarray in an Array

## 1️⃣7️⃣ Pascal’s Triangle

### Core Insight

Each row follows **nCr (binomial coefficients)**.  
Next value can be computed from previous using:

$$ C = C \times \frac{(row-col)}{col} $$

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

**Input:** `5`  
**Output:**  
`[[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]`

---

### Complexity

- Time: **O(n²)**
    
- Space: **O(n²)** (output storage)
    

---

### Interview Notes

- Uses combinatorics instead of DP
    
- `long` prevents overflow during computation
    
- Row always starts and ends with `1`
    
- Pattern: **Combinatorics / Binomial Coefficients**
    

---
## 1️⃣8️⃣ Majority Element II (Boyer–Moore n/3)

### Core Insight

More than ⌊n/3⌋ ⇒ **at most 2 majority elements** can exist.  
So track **two candidates** and cancel others.

---

### Algorithm

**Phase 1: Find candidates**

- Maintain `(el1,cnt1)` and `(el2,cnt2)`
    
- If number matches candidate → increment
    
- If count is 0 → assign new candidate
    
- Otherwise decrement both counts
    

**Phase 2: Verify**

- Count occurrences of candidates
    
- Add those > ⌊n/3⌋
    

---

### Code (Java)

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        int cnt1 = 0, cnt2 = 0;
        int el1 = 0, el2 = 0;

        // Phase 1: candidates
        for (int x : nums) {
            if (el1 == x) cnt1++;
            else if (el2 == x) cnt2++;
            else if (cnt1 == 0) { el1 = x; cnt1 = 1; }
            else if (cnt2 == 0) { el2 = x; cnt2 = 1; }
            else { cnt1--; cnt2--; }
        }

        // Phase 2: verify
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

**Input:** `[1,2,1,2,1,3]`  
**Output:** `[1]`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(1)**
    

---

### Interview Notes

- Extension of Boyer–Moore (n/2 → n/3)
    
- Max 2 valid answers
    
- Second pass is mandatory (no guarantee)
    
- Sorting result not required
    
- Pattern: **Extended Boyer–Moore Voting**
    

---
Key reasoning:

> If 3 numbers each appear > n/3 → total > n → impossible.

This is a **concept test**, not just coding.

---## 1️⃣8️⃣ Majority Element II (Boyer–Moore n/3)

### Core Insight

More than ⌊n/3⌋ ⇒ **at most 2 majority elements** can exist.  
So track **two candidates** and cancel others.

---

### Algorithm

**Phase 1: Find candidates**

- Maintain `(el1,cnt1)` and `(el2,cnt2)`
    
- If number matches candidate → increment
    
- If count is 0 → assign new candidate
    
- Otherwise decrement both counts
    

**Phase 2: Verify**

- Count occurrences of candidates
    
- Add those > ⌊n/3⌋
    

---

### Code (Java)

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        int cnt1 = 0, cnt2 = 0;
        int el1 = 0, el2 = 0;

        // Phase 1: candidates
        for (int x : nums) {
            if (el1 == x) cnt1++;
            else if (el2 == x) cnt2++;
            else if (cnt1 == 0) { el1 = x; cnt1 = 1; }
            else if (cnt2 == 0) { el2 = x; cnt2 = 1; }
            else { cnt1--; cnt2--; }
        }

        // Phase 2: verify
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

**Input:** `[1,2,1,2,1,3]`  
**Output:** `[1]`

---

### Complexity

- Time: **O(n)**
    
- Space: **O(1)**
    

---

### Interview Notes

- Extension of Boyer–Moore (n/2 → n/3)
    
- Max 2 valid answers
    
- Second pass is mandatory (no guarantee)
    
- Sorting result not required
    
- Pattern: **Extended Boyer–Moore Voting**


---
