# Reverse Linked List — LeetCode 206

**Pattern:** Linked List + Iteration + Three Pointers

## 1. Is your code correct?

<badge color="success">Your code is 100% correct</badge>

You have implemented the **iterative approach** using three pointers:

- `curr` → Current node.
- `prev` → Previous node.
- `newnode` → Temporarily stores the next node.

## 2. Understanding your approach

Your code:

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* curr = head;
        ListNode* prev = nullptr;

        while(curr != nullptr) {
            ListNode* newnode = curr->next;
            curr->next = prev;
            prev = curr;
            curr = newnode;
        }

        return prev;
    }
};
```

Let's understand the four important operations:

**Step 1: Store the next node**
```cpp
ListNode* newnode = curr->next;
```
Save the next node because we are going to change `curr->next`.

**Step 2: Reverse the link**
```cpp
curr->next = prev;
```
Make the current node point to the previous node.

**Step 3: Move `prev` forward**
```cpp
prev = curr;
```
The current node becomes the previous node for the next iteration.

**Step 4: Move `curr` forward**
```cpp
curr = newnode;
```
Move to the next node using the address we saved earlier.

## 3. Dry run

Consider:

```text
1 → 2 → 3 → NULL
```

Initially:
```text
prev = NULL
curr = 1
```

| Iteration | Current node | Link reversed | prev | curr |
|---|---|---|---|---|
| 1 | 1 | 1 → NULL | 1 | 2 |
| 2 | 2 | 2 → 1 | 2 | 3 |
| 3 | 3 | 3 → 2 | 3 | NULL |

Final linked list:

```text
3 → 2 → 1 → NULL
```

Return `prev`, because it points to the new head.

## 4. Complexity analysis

- **Time Complexity:** O(N) — Every node is visited exactly once.
- **Auxiliary Space:** O(1) — Only three pointers are used; no extra data structure.

## 5. Important interview takeaway

Remember this sequence:

```cpp
newnode = curr->next;
curr->next = prev;
prev = curr;
curr = newnode;
```

**Why do we need `newnode`?**

Because after reversing `curr->next`, we would lose the original connection to the next node.

This is the standard optimal iterative solution for reversing a singly linked list.
