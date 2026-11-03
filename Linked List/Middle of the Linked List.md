# Middle of the Linked List — LeetCode 876

## Pattern
Linked List + Slow and Fast Pointers (Floyd's Algorithm)

## Approach

We use two pointers:

- `slow` moves one step at a time.
- `fast` moves two steps at a time.

Initially, both point to `head`.

```cpp
ListNode* slow = head;
ListNode* fast = head;
```

Move them using:

```cpp
while(fast != nullptr && fast->next != nullptr) {
    slow = slow->next;
    fast = fast->next->next;
}
```

When `fast` reaches the end of the linked list, `slow` will be at the middle.

**Why does this work?**

Because `fast` moves twice as fast as `slow`.

When `fast` travels `n` nodes, `slow` travels approximately `n/2` nodes.

For an even-length linked list, `slow` reaches the second middle node.

## Code (C++)

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while(fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
        }

        return slow;
    }
};
```

Your original code is correct. The initial condition:

```cpp
if(head == nullptr || head->next == nullptr) return head;
```

is optional because the loop already handles both cases.

## Dry Run 1 — Odd Number of Nodes

```text
1 → 2 → 3 → 4 → 5
```

Initially:

```text
slow = 1
fast = 1
```

| Iteration | Slow | Fast |
|---|---|---|
| 1 | 2 | 3 |
| 2 | 3 | 5 |

Now `fast->next == nullptr`, so the loop stops.

**Answer: Node 3**

## Dry Run 2 — Even Number of Nodes

```text
1 → 2 → 3 → 4 → 5 → 6
```

| Iteration | Slow | Fast |
|---|---|---|
| 1 | 2 | 3 |
| 2 | 3 | 5 |
| 3 | 4 | nullptr |

**Answer: Node 4**

Notice that there are two middle nodes: `3` and `4`.

Since both pointers start at `head`, this implementation returns the **second middle node**.

## What about your commented code?

You don't need this condition:

```cpp
// if(fast->next != nullptr)
```

The loop condition already handles odd and even lengths correctly.

- Odd length: `fast` eventually becomes `nullptr`.
- Even length: `fast->next` eventually becomes `nullptr`.

In both cases, `slow` points to the required middle node.

## Complexity Analysis

- **Time Complexity:** `O(n)` — We traverse approximately half the linked list using the fast pointer.
- **Space Complexity:** `O(1)` — Only two pointers are used.

## Key Takeaways

- Slow pointer moves one step.
- Fast pointer moves two steps.
- Start both pointers at `head` to get the second middle for even-length lists.
- No need to calculate the linked list length.
- No extra data structure is required.

**Your implementation is correct and optimal.**
