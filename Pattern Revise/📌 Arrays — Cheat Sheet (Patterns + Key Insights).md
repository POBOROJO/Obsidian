## 🔹 Basic Manipulation

### **Pattern 1 — Two Pointers / In-place Partition**

**Use when:** sorted array, in-place ops, no extra space  
**Core:** `i → last valid`, `j → scan`

- **Remove Duplicates (Sorted Array)**
    
    - Insight: duplicates are adjacent → overwrite next unique
        
    - Remember: `i = 0`, return `i + 1`
        
- **Move Zeroes**
    
    - Insight: move non-zero forward → zeros auto-shift
        
    - Remember: find first zero index first (else pointer breaks)
        

---

### **Pattern 2 — Rotation via Reversal**

**Use when:** rotate array in-place  
**Core:** reverse whole → reverse parts

- **Rotate Array**
    
    - Insight: 3 reversals = rotation
        
    - Remember: `k %= n`
        
- **Check Sorted + Rotated**
    
    - Insight: exactly **1 break point**
        
    - Remember: include circular check `nums[n-1] > nums[0]`
        

---

### **Pattern 3 — Range Math Trick**

**Use when:** numbers in `[0,n]` or `[1,n]`

- **Missing Number**
    
    - Insight: expected sum − actual sum
        
    - Remember: use XOR if overflow risk
        

---

## 🔹 Hashing & Frequency

### **Pattern 1 — Complement Lookup (HashMap)**

**Use when:** pair/group sum problems

- **Two Sum**
    
    - Insight: check complement before inserting
        
    - Remember: insert AFTER check
        
- **Subarray Sum = K**
    
    - Insight: `prefix - k` lookup
        
    - Remember: `{0 → 1}` initialization
        

---

### **Pattern 2 — Prefix + HashMap**

**Use when:** negatives exist (sliding window fails)

- **Largest Subarray with 0 Sum**
    
    - Insight: same prefix → zero-sum between
        
    - Remember: store FIRST occurrence only
        
- **Subarray XOR = K**
    
    - Insight: `prefix ^ k`
        
    - Remember: `{0 → 1}` again
        

---

## 🔹 Greedy / Kadane

### **Pattern 1 — Running Optimal + Reset**

**Use when:** max/min subarray, profit problems

- **Kadane’s Algorithm**
    
    - Insight: negative sum = dead weight → reset
        
    - Remember: update max BEFORE reset
        
- **Best Time to Buy/Sell Stock**
    
    - Insight: profit = current − min so far
        
    - Remember: update profit BEFORE min
        

---

### **Pattern 2 — Index Mapping**

**Use when:** fixed placement (no swapping)

- **Rearrange by Sign**
    
    - Insight: `+ → even`, `- → odd`
        
    - Remember: `pos = 0`, `neg = 1`, step = 2
        
- **Sort Colors (DNF)**
    
    - Insight: 3 regions (`low`, `mid`, `high`)
        
    - Remember: don’t move `mid` after swapping with `high`
        

---
### **Pattern 3 — Prefix + Suffix Traversal**

**Use when:** product subarray, negatives can flip the answer, zero resets **Core:** scan both ends simultaneously — covers what single direction misses after a flip

- **Maximum Product Subarray**
    - Insight: negative flips min↔max — prefix + suffix together cover all cases
    - Remember: reset to `1` when product hits `0`, not after
---

## 🔹 Sorting-Based Patterns

### **Pattern 1 — Sort + Two Pointers**

**Use when:** multi-sum problems

- **3Sum**
    
    - Insight: reduce to 2-sum
        
    - Remember: skip duplicates at ALL levels
        
- **4Sum**
    
    - Insight: same + extra loop
        
    - Remember: use `long` (overflow kills answers)
        

---

### **Pattern 2 — Intervals (Greedy + Sort)**

**Use when:** overlapping ranges

- **Merge Intervals**
    
    - Insight: compare only with last merged
        
    - Remember: `<=` not `<`
        

---

### **Pattern 3 — Permutations**

**Use when:** next/previous arrangement

- **Next Permutation**
    
    - Insight: pivot → swap → reverse suffix
        
    - Remember: reverse, don’t sort
        

---

## 🔹 Divide & Conquer

### **Pattern 1 — Merge Sort Counting**

**Use when:** pair counting (i < j conditions)

- **Count Inversions**
    
    - Insight: if `left > right`, count all remaining left
        
    - Remember: count BEFORE merge
        
- **Reverse Pairs**
    
    - Insight: `arr[i] > 2 * arr[j]`
        
    - Remember: use `2L` (overflow trap)
        

---

## 🔹 Voting Algorithms

### **Pattern 1 — Boyer-Moore**

**Use when:** majority element

- **Majority (> n/2)**
    
    - Insight: majority survives cancellations
        
    - Remember: verify if not guaranteed
        
- **Majority (> n/3)**
    
    - Insight: max 2 candidates
        
    - Remember: check match BEFORE count reset
        

---

## 🔹 Matrix Problems

### **Pattern 1 — In-place Transform**

**Use when:** square matrix ops

- **Rotate Image**
    
    - Insight: transpose + reverse rows
        
    - Remember: `j = i + 1` in transpose
        
- **Spiral Matrix**
    
    - Insight: shrink boundaries
        
    - Remember: boundary checks prevent duplicates
        

---

## 🔹 Combinatorics / Math

### **Pattern 1 — Equation System**

**Use when:** missing + repeating

- **Missing & Repeating**
    
    - Insight: solve via sum + square sum
        
    - Remember: use `long`
        
- **Pascal’s Triangle**
    
    - Insight: incremental `nCr`
        
    - Remember: multiply before divide → overflow risk
        

---
