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
## 4️⃣ Happy Number

🔗 LeetCode: [https://leetcode.com/problems/happy-number/](https://leetcode.com/problems/happy-number/)

---

### Description

A number is happy if repeatedly replacing it with the sum of squares of its digits eventually reaches `1`. If it loops endlessly without reaching `1`, it's not happy. Return `true` or `false`.

---

### Core Insight

If the number is not happy, the sequence **cycles** — never reaches 1.

Cycle detection = **Floyd's on the digit-square sequence**.

```
Same slow/fast pattern — just applied to a number sequence instead of a list
```

---

### Algorithm

- `slow = n`, `fast = n`
- While `fast != 1`:
    - `slow = digitSquareSum(slow)` — 1 step
    - `fast = digitSquareSum(digitSquareSum(fast))` — 2 steps
    - If `slow == fast` and neither is `1` → cycle → return `false`
- Return `true`

---

### Code (Java)

```java
class Solution {

    int fun(int n) {
        int sum = 0;
        while (n > 0) {
            int d = n % 10;
            n = n / 10;
            sum = sum + (d * d);
        }
        return sum;
    }

    public boolean isHappy(int n) {
        int slow = n;
        int fast = n;

        while (fast != 1) {
            slow = fun(slow);
            fast = fun(fast);
            fast = fun(fast);

            if (slow == fast && slow != 1) {
                return false;
            }
        }
        return true;
    }
}
```

---

### Example

**Input:** `n = 19`

```
1² + 9² = 82
8² + 2² = 68
6² + 8² = 100
1² + 0² + 0² = 1 ✓
```

**Output:** `true`

**Input:** `n = 2` → enters cycle → **Output:** `false`

---

### Complexity

- Time: **O(log n)** per step, bounded number of steps
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Floyd's Cycle Detection on Number Sequence
```

Key condition:

```
Happy → reaches 1. Not happy → enters cycle. Cycle = slow meets fast.
```

---

### Brutal Truth

If you:

- Use HashSet to track seen numbers → works but O(n) space, misses the elegant solution
- Forget `slow != 1` in the cycle check → returns `false` even when both pointers land on `1` simultaneously
- Apply Floyd's Phase 2 (find entrance) → unnecessary here, we only need to detect cycle existence, not where it starts

---

### Extra Insight

Floyd's shows up in 3 linked list problems you've solved — notice the pattern:

```
Linked List Cycle      → detect cycle in list
Find Duplicate Number  → detect cycle in implicit array list + find entrance  
Happy Number           → detect cycle in number sequence
```

Same algorithm, three different disguises. The moment you see "sequence that might loop" — think Floyd's

---

## 5️⃣ Middle of the Linked List

🔗 LeetCode: [https://leetcode.com/problems/middle-of-the-linked-list/](https://leetcode.com/problems/middle-of-the-linked-list/)

---

### Description

Given `head` of a singly linked list, return the middle node. If two middle nodes exist, return the **second** one.

---

### Core Insight

Fast moves 2x speed of slow — when fast hits end, slow is exactly at the middle.

Even length → slow lands on second middle automatically because fast overshoots.

---

### Algorithm

- `slow = head`, `fast = head`
- While `fast != null` and `fast.next != null`:
    - `slow = slow.next`
    - `fast = fast.next.next`
- Return `slow`

---

### Code (Java)

```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}
```

---

### Example

**Input:** `[1,2,3,4,5]` → **Output:** `[3,4,5]` (middle = 3)

**Input:** `[1,2,3,4,5,6]` → **Output:** `[4,5,6]` (second middle = 4)

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Slow & Fast Pointer (Find Middle)
```

Key condition:

```
fast != null && fast.next != null — both checks mandatory or NPE on even length list
```

---

### Brutal Truth

If you:

- Check only `fast != null` → `fast.next.next` throws NPE when fast is at last node
- Check only `fast.next != null` → loop exits one step early, slow lands before middle
- Return `fast` instead of `slow` → fast is at end, not middle
- Count nodes first then traverse to n/2 → works but two passes, O(2n)

---

### Extra Insight

This is a **sub-step** you've already used inside two other problems:

```
Palindrome Linked List   → find middle, then reverse second half
Find Duplicate Number    → same slow/fast idea on array
```

Middle-finding is a building block — not just a standalone problem. In interviews, when you explain Palindrome Linked List, walk through this step explicitly — it shows you understand the composition.

---

## 6️⃣ Linked List Cycle II

🔗 LeetCode: [https://leetcode.com/problems/linked-list-cycle-ii/](https://leetcode.com/problems/linked-list-cycle-ii/)

---

### Description

Given `head` of a linked list, return the **node where the cycle begins**. Return `null` if no cycle. O(1) space required.

---

### Core Insight

Floyd's Phase 1 finds the **meeting point** inside the cycle.

Floyd's Phase 2 finds the **cycle entrance** — reset slow to head, move both one step at a time, they meet exactly at the entrance.

```
Mathematical fact: distance(head → entrance) = distance(meeting point → entrance)
```

---

### Algorithm

- **Phase 1 — Detect cycle:**
    
    - `slow = slow.next`, `fast = fast.next.next`
    - If `slow == fast` → meeting point found
- **Phase 2 — Find entrance:**
    
    - Reset `slow = head`
    - Move both one step at a time
    - Where they meet = cycle start node
- If `fast` hits null → no cycle, return `null`
    

---

### Code (Java)

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) {
                slow = head;

                while (slow != fast) {
                    slow = slow.next;
                    fast = fast.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```

---

### Example

**Input:** `[3,2,0,-4], pos = 1` **Output:** node with value `2` **Explanation:** tail connects back to index 1 — cycle starts at node 2

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Floyd's Cycle Detection — Phase 1 (detect) + Phase 2 (find entrance)
```

Key condition:

```
After meeting: reset slow to head only — fast stays at meeting point, both move 1 step
```

---

### Brutal Truth

If you:

- Reset both pointers to head → defeats the purpose, just re-runs Phase 1
- Move fast 2 steps in Phase 2 → math breaks, they won't meet at entrance
- Return `fast` instead of `slow` → both are same node at meeting, but conceptually return either
- Skip the null check → NPE on lists with no cycle

---

### Extra Insight

Linked List problems using Floyd's so far:

```
Linked List Cycle     → Phase 1 only (does cycle exist?)
Linked List Cycle II  → Phase 1 + Phase 2 (where does it start?)
Find Duplicate Number → same two phases on an array
Happy Number          → Phase 1 only (does sequence cycle?)
```

The pattern to remember:

```
Phase 1 alone  → detect cycle
Phase 1 + 2    → find cycle entrance
```

---
## 7️⃣ Reorder List

🔗 LeetCode: [https://leetcode.com/problems/reorder-list/](https://leetcode.com/problems/reorder-list/)

---

### Description

Given head of a singly linked list `L0 → L1 → … → Ln`, reorder it to `L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …` in-place. No modifying values, only node pointers.

---

### Core Insight

Three steps composed together:

```
Find middle → Reverse second half → Merge two halves alternately
```

Same building blocks as Palindrome Linked List — except instead of comparing, you **interleave**.

---

### Algorithm

- **Step 1 — Find middle:**
    
    - Slow/fast pointers → `slow` lands at middle
    - Cut list: `slow.next = null`
- **Step 2 — Reverse second half:**
    
    - Reverse from `slow.next` onward using `prev/curr`
- **Step 3 — Merge alternately:**
    
    - `first = head`, `second = reversed head`
    - While `second != null`:
        - Save `temp1 = first.next`, `temp2 = second.next`
        - `first.next = second`, `second.next = temp1`
        - Advance: `first = temp1`, `second = temp2`

---

### Code (Java)

```java
class Solution {

    public void reorderList(ListNode head) {

        // If the list has 0 or 1 node, nothing to reorder
        if (head == null || head.next == null)
            return;

        // ==========================
        // STEP 1: Find the middle
        // ==========================

        ListNode slow = head;
        ListNode fast = head;

        // Move slow by 1 step and fast by 2 steps
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // Example:
        // 1 -> 2 -> 3 -> 4 -> 5
        //           ^
        //          slow

        // ==========================
        // STEP 2: Split into two lists
        // ==========================

        // Second half starts after slow
        ListNode second = slow.next; // so 4

        // Cut the list
        slow.next = null;

        // Now we have:
        //
        // First:
        // 1 -> 2 -> 3
        //
        // Second:
        // 4 -> 5

        // ==========================
        // STEP 3: Reverse second half
        // ==========================

        ListNode prev = null;

        while (second != null) {

            // Save next node
            ListNode next = second.next; // 5

            // Reverse the arrow
            second.next = prev; // 5 -> null

            // Move prev forward
            prev = second; // prev = 4

            // Move second forward
            second = next; // second = 5
        }

        // prev is now the head of reversed list
        second = prev; 

        // Now:
        //
        // First:
        // 1 -> 2 -> 3
        //
        // Second:
        // 5 -> 4

        // ==========================
        // STEP 4: Merge alternately
        // ==========================

        ListNode first = head;

        while (second != null) {

            // Save next nodes BEFORE changing pointers
            ListNode temp1 = first.next;
            ListNode temp2 = second.next;

            // Connect first node to second node
            first.next = second;

            // Connect second node back to first list
            second.next = temp1;

            // Move to next nodes
            first = temp1;
            second = temp2;
        }
    }
}
```

---

### Example

**Input:** `[1,2,3,4,5]`

```
Find middle   → [1,2,3] | [4,5]
Reverse 2nd   → [1,2,3] + [5,4]
Merge         → [1,5,2,4,3]
```

**Output:** `[1,5,2,4,3]`

---

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

### Interview Notes

Pattern:

```
Find Middle + Reverse Second Half + Alternate Merge
```

Key condition:

```
Cut the list at middle (slow.next = null) before reversing — else reverse runs into first half
```

---

### Brutal Truth

If you:

- Forget `slow.next = null` → reverse runs through entire list, first half gets corrupted
- Save `second = slow.next` after setting `slow.next = null` → `second` is now null, lost the reference — save it BEFORE cutting
- Merge while `first != null` instead of `second != null` → odd length list, `first` has one extra node, causes NPE when `second` is already exhausted
- Try to use a deque/array → O(n) space, works but misses the point entirely

---

### Extra Insight

This is the **hardest composition** of linked list building blocks you've done:

```
Palindrome Linked List  → find middle + reverse + COMPARE
Reorder List            → find middle + reverse + MERGE
```

Same first two steps, different third step. If you freeze in an interview, ask yourself:

```
"Can I find the middle?" → yes
"Can I reverse a list?"  → yes  
"What do I do with both halves?" → the problem tells you
```

Break it into 3 sub-problems, solve each independently, compose.