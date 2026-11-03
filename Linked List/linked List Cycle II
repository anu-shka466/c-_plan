# Linked List Cycle II — LeetCode 142

## Pattern
Linked List + Two Pointers + Floyd's Cycle Detection Algorithm

## Approach

We use **Floyd's Cycle Detection Algorithm**, also known as the Slow and Fast Pointer approach.

There are two phases:

### Phase 1: Detect whether a cycle exists

Initialize both pointers at `head`.

```cpp
ListNode* slow = head;
ListNode* fast = head;
```

Move:
- `slow` by one node.
- `fast` by two nodes.

```cpp
while(fast != nullptr && fast->next != nullptr) {
    slow = slow->next;
    fast = fast->next->next;

    if(slow == fast) {
        // Cycle detected
    }
}
```

If `fast` or `fast->next` becomes `nullptr`, there is no cycle.

If `slow == fast`, a cycle exists.

**Important:** We compare node addresses, not node values.

### Phase 2: Find the starting node of the cycle

Once both pointers meet inside the cycle:

```cpp
fast = head;

while(slow != fast) {
    slow = slow->next;
    fast = fast->next;
}

return fast;
```

- Move `fast` back to `head`.
- Keep `slow` at the meeting point.
- Move both pointers one step at a time.

They will meet exactly at the **starting node of the cycle**.

## Why does this work?

Let's understand using distances.

Let:

- `L` = distance from head to cycle start.
- `C` = length of the cycle.
- `X` = distance from cycle start to the meeting point.

When slow and fast meet:

- Slow travels: `L + X`
- Fast travels: `2(L + X)`

Fast has travelled an additional number of complete cycle lengths.

Therefore:

\[
2(L+X)-(L+X)=kC
\]

\[
L+X=kC
\]

Rearranging:

\[
L=kC-X
\]

This means the distance from the meeting point to the cycle start, moving forward around the cycle, is equivalent to the distance from the head to the cycle start (modulo the cycle length).

Therefore, when:
- One pointer starts from `head`.
- Another pointer starts from the meeting point.

And both move one step at a time, they meet at the cycle's starting node.

## Code (C++)

```cpp
class Solution {
public:
    ListNode* detectCycle(ListNode* head) {

        if(head == nullptr || head->next == nullptr)
            return nullptr;

        ListNode* slow = head;
        ListNode* fast = head;

        // Phase 1: Detect cycle
        while(fast != nullptr && fast->next != nullptr) {

            slow = slow->next;
            fast = fast->next->next;

            if(slow == fast) {

                // Phase 2: Find cycle starting node
                fast = head;

                while(slow != fast) {
                    slow = slow->next;
                    fast = fast->next;
                }

                return fast;
            }
        }

        return nullptr;
    }
};
```

## Dry Run

**Input:**

```text
head = [3, 2, 0, -4]
pos = 1
```

The last node points back to node `2`.

<text color="secondary" size="xs">Conceptual linked list:</text>

```text
3 → 2 → 0 → -4
    ↑          |
    └──────────┘
```

### Phase 1: Detect cycle

Initially:

```text
slow = 3
fast = 3
```

| Iteration | Slow | Fast |
|---|---|---|
| 1 | 2 | 0 |
| 2 | 0 | 2 |
| 3 | 2 | 2 |

Both pointers meet at node `2`.

### Phase 2: Find cycle start

Move `fast` back to `head`.

```text
slow = 2
fast = 3
```

Move both one step:

| Iteration | Slow | Fast |
|---|---|---|
| 1 | 0 | 2 |
| 2 | -4 | 0 |
| 3 | 2 | -4 |
| 4 | 0 | 2 |

Correction: the pointers meet at node `2` after the first simultaneous move in this example only if the initial meeting point is node `-4`; the table above illustrates why the actual meeting point must be tracked carefully.

For the standard pointer movement, the actual first meeting is node `-4`:

```text
slow = -4
fast = -4
```

Reset `fast = head`:

```text
slow = -4
fast = 3
```

Move both:

```text
slow = 2
fast = 2
```

They meet at node `2`.

**Output:** The node containing `2`.

## Complexity Analysis

- **Time Complexity:** `O(n)` — Both phases take linear time.
- **Space Complexity:** `O(1)` — Only two pointers are used.

## Key Takeaways

- Slow moves one step; fast moves two steps.
- If they meet, a cycle exists.
- Reset one pointer to `head`.
- Move both pointers one step at a time to find the cycle start.
- Compare pointers using `slow == fast`, not their values.
- The linked list is not modified.

**Your implementation is correct and optimal.**
