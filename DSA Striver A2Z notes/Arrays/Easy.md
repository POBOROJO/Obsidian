---
tags:
  - arrays
  - easy
---
## ✅ Problem Tracker - 
- [x] 1️⃣ Check if Array Is Sorted and Rotated
- [x] 2️⃣ Remove Duplicates from Sorted Array 
- [x] 3️⃣ Rotate Array 
- [x] 4️⃣ Move Zeroes
- [x] 5️⃣ Missing Number 

# 1️⃣ Check if Array Is Sorted and Rotated

### Problem

Given an array `nums`, return `true` if it was originally sorted in non-decreasing order and then rotated any number of times (including 0). Otherwise return `false`.

---

### Core Insight

A non-decreasing sorted array rotated any number of times has **at most one inversion** (`nums[i-1] > nums[i]`) in a **circular traversal**.

---

### Algorithm

* Count inversions in linear order
* Also compare `nums[n-1] > nums[0]` for circular break
* If total breaks ≤ 1 → **valid**
* If > 1 → **not valid**

---

### Code (Java)

```java
class Solution {
    public boolean check(int[] nums) {
        int count = 0;
        int n = nums.length;

        for (int i = 1; i < n; i++) {
            if (nums[i - 1] > nums[i]) count++;
        }
        if (nums[n - 1] > nums[0]) count++;

        return count <= 1;
    }
}
```

---

### Example

**Input:** `[3,4,5,1,2]`
**Output:** `true`

---

### Complexity

* Time: **O(n)**
* Space: **O(1)**

---

### Interview Notes

* Duplicates allowed → use `>` not `>=`
* Missing circular check = **wrong**
* Pattern: **Rotated Sorted Array Validation**

---

# 2️⃣ Remove Duplicates from Sorted Array

### Problem

Given a sorted array `nums`, remove duplicates **in-place** such that each unique element appears once. Return the number of unique elements `k`.

---

### Core Insight

Sorted array ⇒ duplicates are adjacent.
Use **two pointers** to overwrite duplicates in-place.

---

### Algorithm

* `i` → last unique index
* `j` → scan array
* On new value: increment `i`, overwrite

---

### Code (Java)

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0) return 0;

        int i = 0;
        for (int j = 1; j < nums.length; j++) {
            if (nums[i] != nums[j]) {
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

**Input:** `[0,0,1,1,2,2,3]`
**Output:** `k = 4`, array → `[0,1,2,3]`

---

### Complexity

* Time: **O(n)**
* Space: **O(1)**

---

### Interview Notes

* Invariant: `nums[0..i]` always unique
* Overwriting equal values is safe
* Pattern: **Two Pointers / In-place Deduplication**

---

# 3️⃣ Rotate Array (Right Rotation)

### Problem

Given an array `nums` and integer `k`, rotate the array to the right by `k` steps in-place.

---

### Core Insight

Right rotation by `k` can be done using **three reversals**.

---

### Algorithm

1. `k = k % n`
2. Reverse entire array
3. Reverse first `k` elements
4. Reverse remaining `n-k`

---

### Code (Java)

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n;

        reverse(nums, 0, n - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, n - 1);
    }

    private void reverse(int[] nums, int l, int r) {
        while (l < r) {
            int temp = nums[l];
            nums[l] = nums[r];
            nums[r] = temp;
            l++;
            r--;
        }
    }
}
```

---

### Example

**Input:** `[1,2,3,4,5,6,7], k = 3`
**Output:** `[5,6,7,1,2,3,4]`

---

### Complexity

* Time: **O(n)**
* Space: **O(1)**

---

### Interview Notes

* Forgetting `k % n` = rookie error
* Mandatory pattern
* Pattern: **Array Rotation via Reversal**

---

# 4️⃣ Move Zeroes

### Problem

Move all `0`s to the end of the array while maintaining the relative order of non-zero elements. Must be done in-place.

---

### Core Insight

Don’t move zeros.
**Shift non-zeros forward**, zeros fall to the end.

---

### Algorithm

* Find first zero index
* Swap subsequent non-zeros forward
* Preserve order (stable)

---

### Code (Java)

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int j = -1;

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                j = i;
                break;
            }
        }
        if (j == -1) return;

        for (int i = j + 1; i < nums.length; i++) {
            if (nums[i] != 0) {
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                j++;
            }
        }
    }
}
```

---

### Example

**Input:** `[0,1,0,3,12]`
**Output:** `[1,3,12,0,0]`

---

### Complexity

* Time: **O(n)**
* Space: **O(1)**

---

### Interview Notes

* Stable partition
* Two-pass but very safe
* Pattern: **Two Pointers / Stable Partition**

---

# 5️⃣ Missing Number

### Problem

Given `n` distinct numbers in range `[0, n]`, return the only missing number.

---

### Core Insight

Numbers are in range `[0,n]`.
Missing number = **expected sum − actual sum**.

---

### Algorithm

* Compute `n*(n+1)/2`
* Subtract array sum

---

### Code (Java)

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int expected = n * (n + 1) / 2;
        int actual = 0;

        for (int x : nums) actual += x;
        return expected - actual;
    }
}
```

---

### Example

**Input:** `[3,0,1]`
**Output:** `2`

---

### Complexity

* Time: **O(n)**
* Space: **O(1)**

---

### Interview Notes

* XOR version avoids overflow
* Pattern: **Math / Range Sum Difference**

---

