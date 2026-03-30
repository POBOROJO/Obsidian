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