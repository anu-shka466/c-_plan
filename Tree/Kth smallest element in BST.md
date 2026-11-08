# Kth Smallest Element in a BST — LeetCode 230

**Pattern:** BST + Iterative Inorder Traversal (Stack)

## 1. Is your code correct?

<badge color="success">Your updated code is correct</badge>

Your approach works because inorder traversal of a BST visits nodes in ascending sorted order.

You have correctly fixed the earlier mistakes:
- Removed the incorrect `while` condition.
- Decreased `k` after visiting each node.
- Returned the value when `k == 0`.

## 2. Your code with one small improvement

You don't need the special single-node condition. The loop already handles it.

```cpp
class Solution {
public:
    void pushleft(TreeNode* root, stack<TreeNode*>& st) {
        while(root != nullptr) {
            st.push(root);
            root = root->left;
        }
    }

    TreeNode* after(stack<TreeNode*>& st) {
        TreeNode* node = st.top();
        st.pop();

        pushleft(node->right, st);

        return node;
    }

    int kthSmallest(TreeNode* root, int k) {
        stack<TreeNode*> st;
        pushleft(root, st);

        while(!st.empty()) {
            TreeNode* node = after(st);
            k--;

            if(k == 0) {
                return node->val;
            }
        }

        return -1;
    }
};
```

## 3. Dry run

Consider this BST:

```text
        5
       / \
      3   6
     / \
    2   4
   /
  1
```

Suppose `k = 3`.

Inorder traversal:

`1 → 2 → 3 → 4 → 5 → 6`

| Visited node | k before | k after |
|---|---:|---:|
| 1 | 3 | 2 |
| 2 | 2 | 1 |
| 3 | 1 | 0 |

When `k == 0`, return `3`.

## 4. Complexity

- **Time:** O(H + k), where H is the tree height. Worst case O(N).
- **Auxiliary space:** O(H), for the stack.

## 5. Interview takeaway

Remember this relationship:

- BST inorder traversal = sorted ascending order.
- `pushleft()` initializes the smallest available node.
- `after()` moves to the next smallest node.
- Decrement `k` after every visited node.
- Return when `k` becomes zero.

**Your iterative inorder approach is a good interview solution because it avoids storing the entire inorder traversal.**
