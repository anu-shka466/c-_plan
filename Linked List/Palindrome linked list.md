# Palindrome Linked List — LeetCode 234

## Pattern
Linked List + Slow and Fast Pointers + In-place Reversal

## Approach

We can solve this problem in three steps without using an extra array or stack.

### Step 1: Find the middle of the linked list

Use slow and fast pointers:

- `slow` moves one step.
- `fast` moves two steps.

```cpp
while(fast != nullptr && fast->next != nullptr) {
    slow = slow->next;
    fast = fast->next->next;
}
```

When the loop finishes:
- For an even-length list, `slow` points to the first node of the second half.
- For an odd-length list, `slow` points to the middle node.

### Step 2: Skip the middle node if the length is odd

```cpp
if(fast != nullptr) {
    slow = slow->next;
}
```

Why?

If `fast != nullptr`, the linked list has an odd number of nodes.

Example:

```text
1 → 2 → 3 → 2 → 1
         ↑
        slow
```

The middle node `3` does not need to be compared.

So we move `slow` to the next node.

### Step 3: Reverse the second half

Reverse the linked list starting from `slow`.

```cpp
ListNode* prev = nullptr;

while(slow != nullptr) {
    ListNode* newnode = slow->next;
    slow->next = prev;
    prev = slow;
    slow = newnode;
}
```

After reversal, `prev` points to the new head of the reversed second half.

### Step 4: Compare both halves

```cpp
while(prev != nullptr) {
    if(prev->val != head->val)
        return false;

    prev = prev->next;
    head = head->next;
}
```

Compare corresponding node values.

If any two values differ, the linked list is not a palindrome.

Otherwise, return `true`.

## Code (C++)

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {

        if(head == nullptr || head->next == nullptr)
            return true;

        // Step 1: Find middle
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // Step 2: Skip middle if odd length
        if(fast != nullptr) {
            slow = slow->next;
        }

        // Step 3: Reverse second half
        ListNode* prev = nullptr;

        while(slow != nullptr) {
            ListNode* newnode = slow->next;
            slow->next = prev;
            prev = slow;
            slow = newnode;
        }

        // Step 4: Compare both halves
        while(prev != nullptr) {
            if(prev->val != head->val)
                return false;

            prev = prev->next;
            head = head->next;
        }

        return true;
    }
};
```

Your implementation follows the correct optimal approach.

## Dry Run 1 — Even Length

**Input:**

```text
1 → 2 → 2 → 1
```

### Step 1: Find middle

Initially:

```text
slow = 1
fast = 1
```

After first iteration:

```text
slow = 2
fast = 2 (third node)
```

After second iteration:

```text
slow = 2 (third node)
fast = nullptr
```

Since `fast == nullptr`, the list has an even number of nodes.

We don't skip `slow`.

### Step 2: Reverse second half

Before:

```text
1 → 2 → 2 → 1
        ↑
       slow
```

After reversal:

```text
1 → 2 → 2 ← 1
```

The reversed second half starts at `prev`:

```text
1 → 2
```

### Step 3: Compare

| First half | Reversed second half | Match |
|---|---|---|
| 1 | 1 | Yes |
| 2 | 2 | Yes |

Output:

```cpp
true
```

## Dry Run 2 — Odd Length

**Input:**

```text
1 → 2 → 3 → 2 → 1
```

After finding the middle:

```text
slow = 3
fast = 1 (last node)
```

Since `fast != nullptr`, skip the middle node.

```text
slow = 2 (fourth node)
```

Reverse the second half:

```text
1 → 2
```

Compare:

```text
First half:             1 → 2
Reversed second half:   1 → 2
```

Output:

```cpp
true
```

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - Finding middle: `O(n)`
  - Reversing second half: `O(n)`
  - Comparing both halves: `O(n)`

- **Space Complexity:** `O(1)`
  - Only pointers are used.
  - No additional array, stack, or linked list is created.

## Key Takeaways

- Slow and fast pointers find the middle.
- `fast != nullptr` identifies an odd-length list.
- Skip the middle node for odd-length lists.
- Reverse the second half in-place.
- Compare the first half with the reversed second half.
- This is an optimal `O(n)` time and `O(1)` extra-space solution.

**Your implementation is correct.**

**One important interview note:** Your code modifies the original linked list by reversing its second half. If the interviewer expects the original list to remain unchanged, reverse the second half again before returning.
