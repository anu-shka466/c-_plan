# Rotate List — LeetCode 61

## Pattern
Linked List + Pointer Manipulation

## Approach

We need to rotate the linked list to the right by `k` positions.

1. Handle edge cases:
   - If the list is empty, has only one node, or `k == 0`, return `head`.
2. Calculate the length of the linked list.
3. Since rotating by the list length brings us back to the original list, calculate `k = k % len`.
4. If `k == 0`, return the original list.
5. Find the new tail at position `len - k` and the new head at position `len - k + 1`.
6. Connect the original tail to the original head.
7. Disconnect the new tail from the new head.
8. Return the new head.

## C++ Code

```cpp
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {

        if (k == 0 || head == nullptr || head->next == nullptr)
            return head;

        int len = 0;
        ListNode* temp = head;

        // Calculate length
        while (temp != nullptr) {
            len++;
            temp = temp->next;
        }

        // Remove unnecessary rotations
        k = k % len;

        if (k == 0) return head;

        ListNode* prev = nullptr;
        ListNode* curr = head;

        // Find the new head
        for (int i = 0; i < len - k; i++) {
            prev = curr;
            curr = curr->next;
        }

        // Find original tail
        temp = curr;
        while (temp->next != nullptr) {
            temp = temp->next;
        }

        // Reconnect the list
        temp->next = head;
        prev->next = nullptr;

        return curr;
    }
};
```

## Dry Run

Input:

```text
head = 1 → 2 → 3 → 4 → 5
k = 2
```

**Step 1: Calculate length**

```text
len = 5
```

**Step 2: Calculate effective rotation**

```text
k = k % len
k = 2 % 5 = 2
```

**Step 3: Find the new head**

We calculate:

```text
len - k = 5 - 2 = 3
```

After moving three times:

```text
prev = 3
curr = 4
```

So:
- New tail = `3`
- New head = `4`

**Step 4: Connect original tail to original head**

```text
5 → 1
```

**Step 5: Disconnect the new tail**

```cpp
prev->next = nullptr;
```

Final list:

```text
4 → 5 → 1 → 2 → 3
```

Output: `[4,5,1,2,3]`

## Complexity

- **Time:** `O(n)` — calculating length, finding the new head, and finding the original tail.
- **Space:** `O(1)` — only a constant number of pointers are used.

## Key Takeaways

- `k % len` avoids unnecessary rotations.
- The new head is at position `len - k + 1` (1-indexed).
- The new tail is at position `len - k`.
- Connect the original tail to the original head.
- Break the link after the new tail.
- Your approach is correct.

## Important Edge Cases

| Input | Output |
|---|---|
| `[1,2,3,4,5], k = 2` | `[4,5,1,2,3]` |
| `[1,2,3], k = 3` | `[1,2,3]` |
| `[1,2], k = 1` | `[2,1]` |
| `[1], k = 10` | `[1]` |
| `[], k = 5` | `[]` |
