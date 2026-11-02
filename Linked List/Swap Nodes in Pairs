# Swap Nodes in Pairs — LeetCode 24

## Pattern
Linked List + Pointer Manipulation

## Approach

We need to swap every two adjacent nodes without changing their values.

1. If the list is empty or has only one node, return `head`.
2. Create a dummy node and connect it to `head`.
3. Maintain two pointers:
   - `prev`: node before the current pair.
   - `curr`: first node of the current pair.
4. Identify the second node using `newnode = curr->next`.
5. Perform three pointer changes:
   - Connect `curr` to the node after the pair.
   - Connect `newnode` to `curr`.
   - Connect `prev` to `newnode`.
6. Move `prev` and `curr` forward to process the next pair.
7. Return `dummy->next`.

## C++ Code

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == nullptr || head->next == nullptr)
            return head;

        ListNode* dummy = new ListNode(0);
        dummy->next = head;

        ListNode* prev = dummy;
        ListNode* curr = head;

        while (curr != nullptr && curr->next != nullptr) {

            ListNode* newnode = curr->next;

            curr->next = newnode->next;
            newnode->next = curr;
            prev->next = newnode;

            prev = curr;
            curr = curr->next;
        }

        return dummy->next;
    }
};
```

## Dry Run

Input:

```text
1 → 2 → 3 → 4
```

Initially:

```text
dummy → 1 → 2 → 3 → 4
         ↑   ↑
        curr newnode
```

**First iteration:**

```cpp
newnode = curr->next;       // newnode = 2
curr->next = newnode->next; // 1 → 3
newnode->next = curr;       // 2 → 1
prev->next = newnode;       // dummy → 2
```

List becomes:

```text
dummy → 2 → 1 → 3 → 4
```

Move pointers:

```text
prev = 1
curr = 3
```

**Second iteration:**

Swap `3` and `4`.

```text
dummy → 2 → 1 → 4 → 3
```

Now `curr = nullptr`, so the loop stops.

Output: `[2,1,4,3]`

## Complexity

- **Time:** `O(n)` — each pair is processed once.
- **Space:** `O(1)` — only a constant number of pointers are used.

## Key Takeaways

- `dummy` handles swapping when the first pair includes the head.
- `prev` maintains the connection to the previous part of the list.
- `curr` is the first node of the pair.
- `newnode` is the second node of the pair.
- We modify only `next` pointers, never node values.
- If the list has an odd number of nodes, the last node remains unchanged.
- Your pointer logic is correct.

**Small C++ improvement:** You can use a stack-allocated dummy node to avoid dynamic allocation:

```cpp
ListNode dummy(0);
dummy.next = head;
ListNode* prev = &dummy;
```

The rest of the algorithm remains the same.
