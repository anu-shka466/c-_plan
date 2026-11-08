# Two Sum IV — Input is a BST — LeetCode 653

## Pattern
Binary Search Tree + Two Pointers + Stack (BST Iterators)

## Is your code correct?
**Yes, your approach is correct.** ✅

You are using an optimized two-pointer technique on a BST without converting the tree into an array.

You maintain two stacks:

- `s1` → Inorder traversal (smallest to largest).
- `s2` → Reverse inorder traversal (largest to smallest).

This is similar to the two-pointer approach used in a sorted array.

## Approach

### 1. `pushleft()`

```cpp
void pushleft(TreeNode* root, stack<TreeNode*>& st) {
    while(root != nullptr) {
        st.push(root);
        root = root->left;
    }
}
```

Pushes the path of left children into the stack.

This helps us get the **smallest available element**.

### 2. `pushright()`

```cpp
void pushright(TreeNode* root, stack<TreeNode*>& st) {
    while(root != nullptr) {
        st.push(root);
        root = root->right;
    }
}
```

Pushes the path of right children into the stack.

This helps us get the **largest available element**.

### 3. `after()`

```cpp
TreeNode* after(stack<TreeNode*>& st) {
    TreeNode* node = st.top();
    st.pop();

    pushleft(node->right, st);
    return node;
}
```

Returns the next greater node in inorder traversal.

### 4. `before()`

```cpp
TreeNode* before(stack<TreeNode*>& st) {
    TreeNode* node = st.top();
    st.pop();

    pushright(node->left, st);
    return node;
}
```

Returns the next smaller node in reverse inorder traversal.

## Your Code

```cpp
class Solution {
public:
    void pushleft(TreeNode* root, stack<TreeNode*>& st) {
        while(root != nullptr) {
            st.push(root);
            root = root->left;
        }
    }

    void pushright(TreeNode* root, stack<TreeNode*>& st) {
        while(root != nullptr) {
            st.push(root);
            root = root->right;
        }
    }

    TreeNode* after(stack<TreeNode*>& st) {
        TreeNode* node = st.top();
        st.pop();

        pushleft(node->right, st);
        return node;
    }

    TreeNode* before(stack<TreeNode*>& st) {
        TreeNode* node = st.top();
        st.pop();

        pushright(node->left, st);
        return node;
    }

    bool findTarget(TreeNode* root, int k) {
        if(root == nullptr ||
           (root->left == nullptr && root->right == nullptr))
            return false;

        stack<TreeNode*> s1, s2;

        pushleft(root, s1);
        pushright(root, s2);

        while(!s1.empty() && !s2.empty()) {
            TreeNode* left = s1.top();
            TreeNode* right = s2.top();

            if(left->val >= right->val)
                break;

            long long sum = (long long)left->val + right->val;

            if(sum == k)
                return true;

            else if(sum < k)
                after(s1);
            else
                before(s2);
        }

        return false;
    }
};
```

## Dry Run

Consider this BST:

          5
         / \
        3   6
       / \   \
      2   4   7

Given:

```cpp
k = 9;
```

**Step 1: Initialize both stacks**

Inorder traversal:

```cpp
2, 3, 4, 5, 6, 7
```

Reverse inorder traversal:

```cpp
7, 6, 5, 4, 3, 2
```

Initially:

```cpp
left  = 2;
right = 7;
```

**Step 2: Calculate sum**

```cpp
2 + 7 = 9
```

Since the sum equals `k`:

```cpp
return true;
```

Final output:

```cpp
true
```

### Another example: k = 10

| Left pointer | Right pointer | Sum | Action |
|---:|---:|---:|---|
| 2 | 7 | 9 | Move left forward |
| 3 | 7 | 10 | Found |

Output:

```cpp
true
```

## Why do we use `left->val >= right->val`?

```cpp
if(left->val >= right->val) break;
```

We must use two different nodes.

- If both pointers reach the same node, we cannot use that node twice.
- If the left pointer crosses the right pointer, all possible pairs have been checked.

This condition also works when the BST contains duplicate values because the nodes are compared by their values and the pointers advance independently.

## Complexity Analysis

Let N be the number of nodes and H be the height of the BST.

- **Time Complexity:** O(N) — each iterator visits each node at most once.
- **Auxiliary Space:** O(H) — each stack stores a path through the tree.

For a balanced BST: O(log N) auxiliary space.

For a skewed BST: O(N) auxiliary space.

## Key Takeaways

- Inorder traversal of BST gives ascending values.
- Reverse inorder traversal gives descending values.
- Two stacks allow us to move from both ends without storing all nodes.
- If sum is smaller than `k`, move the smaller pointer forward.
- If sum is greater than `k`, move the larger pointer backward.
- Use `long long` for safe addition.

## Important Pattern

| Sorted Array | BST |
|---|---|
| Left pointer starts at beginning | Inorder iterator |
| Right pointer starts at end | Reverse inorder iterator |
| Move left when sum is small | `after(s1)` |
| Move right when sum is large | `before(s2)` |

**Remember:** BST Two Sum = Inorder + Reverse Inorder + Two Pointers using Stacks.
