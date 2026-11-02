# Linked List Cycle — LeetCode 141

## Pattern
- Fast & Slow Pointers (Floyd's Cycle Detection Algorithm)

## Approach
1. Initialize two pointers, `slow` and `fast`, at the head.
2. Move `slow` by one node.
3. Move `fast` by two nodes.
4. If `slow == fast`, a cycle exists. Return `true`.
5. If `fast` or `fast->next` becomes `nullptr`, the list has ended. Return `false`.

## Code (C++)
```cpp
class Solution {
public:
    bool hasCycle(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;

        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;

            if (slow == fast) return true;
        }

        return false;
    }
};
```

## Dry Run

Example: `1 -> 2 -> 3 -> 4 -> 2 ...`

| Iteration | Slow | Fast |
|---|---|---|
| Initial | 1 | 1 |
| 1 | 2 | 3 |
| 2 | 3 | 2 |
| 3 | 4 | 4 |

Since `slow == fast`, return `true`.

## Complexity
- **Time:** O(n) — each pointer traverses the list at most a linear number of nodes.
- **Space:** O(1) — only two pointers are used.

## Key Takeaways
- Your code is **correct and optimal**.
- We compare node addresses (`slow == fast`), not node values.
- `fast` moves two steps, while `slow` moves one step.
- The loop condition prevents accessing `fast->next->next` when the list ends.
- Even if the cycle starts somewhere in the middle, the two pointers will eventually meet.
