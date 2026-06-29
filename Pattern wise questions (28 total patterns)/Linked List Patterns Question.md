**NOTE ❗❗❗ — When is a question of Linked List***
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