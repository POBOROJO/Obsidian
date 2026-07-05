**NOTE ❗❗❗ — When is a question of Linked List**
----
## **Tags: Slow & Fast Pointer**

1. **Cyclic / Loop / Repetitive/ Starting point of Cycle**

---

## 1️⃣ Linked List Cycle

🔗 LeetCode: [https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/)

---

### Description

Given `head` of a linked list, determine if it has a cycle. Return `true` if yes, `false` otherwise. Solve in **O(1)** space.

---

### Core Insight

If there's a cycle, a fast pointer moving 2 steps will eventually lap a slow pointer moving 1 step — they will **meet**.

If no cycle, fast pointer hits `null`.

```
Floyd's Cycle Detection (Tortoise and Hare)
```

---

### Algorithm

- `slow = head`, `fast = head`
- While `fast != null` and `fast.next != null`:
    - `slow = slow.next`
    - `fast = fast.next.next`
    - If `slow == fast` → cycle detected, return `true`
- Return `false`

---

### Code (Java)

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) return true;
        }
        return false;
    }
}
```

---

### Example

**Input:** `[3,2,0,-4], pos = 1` **Output:** `true` **Explanation:** tail connects back to index 1 — cycle exists

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Slow & Fast Pointer (Floyd's Cycle Detection)
```

Key condition:

```
Cycle exists → fast eventually laps slow → they meet
No cycle → fast hits null
```

---

### Brutal Truth

If you:

- Check `fast == null` only, not `fast.next == null` → `fast.next.next` throws NullPointerException
- Compare `slow.val == fast.val` instead of `slow == fast` → wrong, different nodes can have same value
- Use HashSet to track visited nodes → works but O(n) space, misses the follow-up entirely

---

### ALGO TO REMEMBER — Floyd's Cycle Detection

**What it does:** detects cycle in linked list in O(n) time, O(1) space

**Hook:** _"Tortoise and Hare — if they're on a circular track, the hare always laps the tortoise"_

**Core steps:**

1. Both start at head
2. Slow moves 1 step, fast moves 2 steps
3. If they meet → cycle
4. If fast hits null → no cycle

---
## 2️⃣ Palindrome Linked List

🔗 LeetCode: [https://leetcode.com/problems/palindrome-linked-list/](https://leetcode.com/problems/palindrome-linked-list/)

---

### Description

Given the `head` of a singly linked list, return `true` if it is a palindrome, `false` otherwise. Solve in **O(n)** time and **O(1)** space.

---

### Core Insight

Can't traverse backward in a singly linked list — so **reverse the second half in-place** and compare against the first half.

Three steps:

```
Find middle → Reverse second half → Compare both halves
```

---

### Algorithm

- **Step 1 — Find middle:**
    
    - `slow = head`, `fast = head`
    - Move until `fast` hits end → `slow` is at middle
- **Step 2 — Reverse second half:**
    
    - Start from `slow`, reverse in-place using `prev/curr`
- **Step 3 — Compare:**
    
    - `p1 = head`, `p2 = prev` (head of reversed half)
    - Walk both until `p2` is null
    - Any mismatch → `false`

---

### Code (Java)

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) return true;

        // Step 1: Find middle
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // Step 2: Reverse second half
        ListNode prev = null;
        ListNode curr = slow;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }

        // Step 3: Compare
        ListNode p1 = head;
        ListNode p2 = prev;
        while (p2 != null) {
            if (p1.val != p2.val) return false;
            p1 = p1.next;
            p2 = p2.next;
        }
        return true;
    }
}
```

---

### Example

**Input:** `[1,2,2,1]` **Output:** `true`

**Input:** `[1,2]` **Output:** `false`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Slow & Fast (Find Middle) + In-place Reverse + Two Pointer Compare
```

Key condition:

```
Reverse second half only — compare p2 until null, not p1 (handles odd-length lists)
```

---

### Brutal Truth

If you:

- Use a stack or array to store values → O(n) space, fails the follow-up
- Compare until `p1 == null` instead of `p2 == null` → fails on odd-length lists, middle element has no pair
- Forget the `head == null || head.next == null` guard → NPE on empty or single node list
- Reverse the whole list instead of second half → destroys the first half, nothing to compare against

---

### Extra Insight

This problem combines **3 linked list sub-patterns** in sequence:

```
1. Find middle        → slow/fast pointer
2. Reverse a list     → prev/curr/next pointer
3. Compare two lists  → two pointer walk
```

Each of these is its own interview question too. Knowing all three and composing them is the real skill being tested here.

---

![[Pasted image 20260703013208.png]]

---
## 3️⃣ Find the Duplicate Number

🔗 LeetCode: [https://leetcode.com/problems/find-the-duplicate-number/](https://leetcode.com/problems/find-the-duplicate-number/)

---

### Description

Given array `nums` of `n+1` integers where each integer is in range `[1,n]`, find the one repeated number. Cannot modify array, must use O(1) space.

---

### Core Insight

Treat the array as a **linked list where index → value is a "next" pointer**.

Duplicate value = two indices point to the same next node = **cycle in the implicit linked list**.

Apply Floyd's — find meeting point, then find cycle entrance.

```
Same algo as Linked List Cycle II — just on an array
```

---

### Algorithm

- **Phase 1 — Find meeting point:**
    
    - `slow = nums[slow]`, `fast = nums[nums[fast]]`
    - Loop until `slow == fast`
- **Phase 2 — Find cycle entrance (= duplicate):**
    
    - Reset `slow = 0` (start of array)
    - Move both one step at a time
    - Where they meet = duplicate number

---

### Code (Java)

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = 0;
        int fast = 0;

        // Phase 1: find meeting point
        while (true) {
            slow = nums[slow];
            // `fast = nums[nums[fast]]` = below one
            fast = nums[fast];
            fast = nums[fast];

            if (slow == fast) break;
        }

        // Phase 2: find cycle entrance
        slow = 0;

        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
    }
}
```

---

### Example

**Input:** `[1,3,4,2,2]`

Implicit links: `0→1→3→2→4→2` (cycle at 2)

**Output:** `2`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Floyd's Cycle Detection on Array (Implicit Linked List)
```

Key condition:

```
Values in [1,n] guarantee every index has a valid next → no null, cycle must exist
```

---

### Brutal Truth

If you:

- Use HashSet to track seen values → O(n) space, violates constraint
- Sort the array → modifies array, violates constraint
- Use `nums[i]` as index and mark negative → modifies array, violates constraint
- Start `slow` and `fast` at `nums[0]` instead of `0` → Phase 2 reset to `0` (not `nums[0]`) is the trap — must reset to start of the "list" which is index `0`
- Don't understand why Phase 2 reset works → distance from head to cycle entrance equals distance from meeting point to cycle entrance (mathematical proof, just remember the fact)

---

### Extra Insight

Why reset `slow = 0` not `slow = nums[0]`:

```
Index 0 is the "head" of the implicit linked list
nums[0] would be one step inside — that's wrong
The reset must go back to the true start
```

This is the #1 mistake on this problem. The meeting in Phase 2 happens at the **duplicate value** which is the cycle entrance.

---