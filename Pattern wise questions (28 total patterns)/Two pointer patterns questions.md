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
