# Reorder List — LeetCode 143

## Pattern
Linked List + Slow and Fast Pointers + Reversal + Merging

## Approach

We solve this problem in three steps:

1. Find the middle of the linked list.
2. Reverse the second half.
3. Merge both halves alternatively.

Your implementation follows exactly this approach and is correct.

### Step 1: Find the middle

Use slow and fast pointers.

```cpp
ListNode* slow = head;
ListNode* fast = head;

while(fast != nullptr && fast->next != nullptr) {
    slow = slow->next;
    fast = fast->next->next;
}
```

For an even-length list:

```text
1 → 2 → 3 → 4 → 5 → 6
        ↑
       slow
```

Here, `slow` points to the first half's last node (`3`).

### Step 2: Separate and reverse the second half

First, separate the two halves:

```cpp
fast = slow->next;
slow->next = nullptr;
```

Now:

```text
First half:   1 → 2 → 3

Second half:  4 → 5 → 6
```

Reverse the second half:

```cpp
ListNode* prev = nullptr;

while(fast != nullptr) {
    ListNode* newnode = fast->next;
    fast->next = prev;
    prev = fast;
    fast = newnode;
}
```

After reversal:

```text
First half:   1 → 2 → 3

Second half:  6 → 5 → 4
```

`prev` points to the head of the reversed second half.

### Step 3: Merge both halves alternatively

We use two pointers:

- `temp1` points to the first half.
- `temp2` points to the reversed second half.

```cpp
while(temp2 != nullptr) {

    ListNode* first = temp1->next;
    ListNode* sec = temp2->next;

    temp1->next = temp2;
    temp2->next = first;

    temp1 = first;
    temp2 = sec;
}
```

The important part is saving the next nodes before changing links.

```cpp
ListNode* first = temp1->next;
ListNode* sec = temp2->next;
```

Otherwise, we could lose access to the remaining nodes.

## Code (C++)

```cpp
class Solution {
public:
    void reorderList(ListNode* head) {

        if(head == nullptr || head->next == nullptr)
            return;

        // Step 1: Find middle
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // Step 2: Separate second half
        fast = slow->next;
        slow->next = nullptr;

        // Reverse second half
        ListNode* prev = nullptr;

        while(fast != nullptr) {
            ListNode* newnode = fast->next;
            fast->next = prev;
            prev = fast;
            fast = newnode;
        }

        // Step 3: Merge both halves
        ListNode* temp1 = head;
        ListNode* temp2 = prev;

        while(temp2 != nullptr) {

            ListNode* first = temp1->next;
            ListNode* sec = temp2->next;

            temp1->next = temp2;
            temp2->next = first;

            temp1 = first;
            temp2 = sec;
        }
    }
};
```

## Dry Run

**Input:**

```text
1 → 2 → 3 → 4 → 5
```

### Step 1: Find middle

After slow and fast pointer traversal:

```text
slow = 3
```

### Step 2: Separate and reverse

Separate:

```text
First:  1 → 2 → 3

Second: 4 → 5
```

Reverse second half:

```text
First:  1 → 2 → 3

Second: 5 → 4
```

### Step 3: Merge

Initially:

```text
temp1 = 1
temp2 = 5
```

**Iteration 1:**

Save next nodes:

```text
first = 2
sec = 4
```

Connect:

```text
1 → 5 → 2
```

Move pointers:

```text
temp1 = 2
temp2 = 4
```

**Iteration 2:**

Save next nodes:

```text
first = 3
sec = nullptr
```

Connect:

```text
2 → 4 → 3
```

Move pointers:

```text
temp1 = 3
temp2 = nullptr
```

Loop stops.

**Final Output:**

```text
1 → 5 → 2 → 4 → 3
```

## Complexity Analysis

- **Time Complexity:** `O(n)`
  - Finding middle: `O(n)`
  - Reversing second half: `O(n)`
  - Merging both halves: `O(n)`

- **Space Complexity:** `O(1)`
  - Only pointers are used.
  - No additional linked list or array is created.

## Key Takeaways

- Find the middle using slow and fast pointers.
- Break the list into two halves using `slow->next = nullptr`.
- Reverse the second half.
- Merge the two halves alternatively.
- Save the next pointers before changing links.
- We modify only node connections, not node values.

**Your implementation is correct and optimal.**

One small improvement: add the `head == nullptr || head->next == nullptr` check to handle empty and single-node lists explicitly.
