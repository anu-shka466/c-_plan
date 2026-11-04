# Reverse Linked List II — LeetCode 92

## Pattern
Linked List + Pointer Manipulation

## Approach

We reverse only the nodes between positions `left` and `right`.

1. Initialize `prev = nullptr` and `curr = head`.
2. Move `curr` to position `left`. `prev` will point to the node just before `left`.
3. Store two pointers:
   - `before = prev`: node before the reversal section.
   - `after = curr`: original first node of the section. After reversal, it becomes the last node.
4. Reverse the links between positions `left` and `right` using the standard linked-list reversal technique.
5. Connect the reversed section:
   - If `before` exists, connect `before->next = prev`.
   - Otherwise, update `head = prev`.
6. Connect the original first node to the remaining list using `after->next = curr`.

## C++ Code

```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode* prev = nullptr;
        ListNode* curr = head;

        // Move to the left position
        for (int i = 1; i < left; i++) {
            prev = curr;
            curr = curr->next;
        }

        // Store connection pointers
        ListNode* before = prev;
        ListNode* after = curr;

        // Reverse the required section
        for (int i = left; i <= right; i++) {
            ListNode* newnode = curr->next;

            curr->next = prev;
            prev = curr;
            curr = newnode;
        }

        // Connect the reversed section
        if (before != nullptr) {
            before->next = prev;
        } else {
            head = prev;
        }

        after->next = curr;

        return head;
    }
};
```

## Dry Run

Input:

```text
head = 1 → 2 → 3 → 4 → 5
left = 2, right = 4
```

**Step 1: Reach position `left = 2`**

```text
prev = 1
curr = 2
```

**Step 2: Store pointers**

```text
before = 1
after  = 2
```

**Step 3: Reverse nodes from position 2 to 4**

Initially:

```text
1 → 2 → 3 → 4 → 5
```

After reversing:

```text
1 ← 2 ← 3 ← 4    5
```

Now:
- `prev = 4`
- `curr = 5`

**Step 4: Reconnect the list**

```cpp
before->next = prev;  // 1 → 4
after->next = curr;   // 2 → 5
```

Final list:

```text
1 → 4 → 3 → 2 → 5
```

Output: `[1,2,3,4,5]` → `[1,4,3,2,5]`

## Complexity

- **Time:** `O(n)` — traverse the list and reverse the required section.
- **Space:** `O(1)` — only a constant number of pointers are used.

## Key Takeaways

- `before` stores the node before the reversal section.
- `after` stores the original first node of the section.
- During reversal, `prev` becomes the new head of the reversed section.
- `curr` points to the node immediately after the reversed section.
- Special case: when `left == 1`, update `head = prev`.
- Your pointer logic is correct for the given constraints.
