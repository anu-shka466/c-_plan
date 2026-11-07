# Binary Tree Inorder Traversal — LeetCode 94

**Pattern:** Binary Tree + DFS + Recursion

## 1. Is your code correct?

<badge color="success">Correct</badge>

Your code is completely correct. You are implementing **recursive inorder traversal**.

Inorder traversal follows this order:

**Left → Root → Right**

## 2. Approach

For every node:

1. First, recursively visit its left subtree.
2. Then, store the current node's value in `ans`.
3. Finally, recursively visit its right subtree.

**Base case:**

```cpp
if(root == nullptr) {
    return;
}
```

If the current node is `nullptr`, there is nothing to process, so we return.

## 3. Code

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {

        vector<int> ans;

        inorder(root, ans);

        return ans;
    }

    void inorder(TreeNode* root, vector<int>& ans) {

        if(root == nullptr) {
            return;
        }

        // 1. Left
        inorder(root->left, ans);

        // 2. Root
        ans.push_back(root->val);

        // 3. Right
        inorder(root->right, ans);
    }
};
```

## 4. Dry Run

Input:

```text
       1
        \
         2
        /
       3
```

Initially:

```cpp
inorder(root, ans);
```

Execution:

| Step | Operation | ans |
|---|---|---|
| 1 | Visit left of 1 (`nullptr`) | [] |
| 2 | Add 1 | [1] |
| 3 | Visit right of 1 (node 2) | [1] |
| 4 | Visit left of 2 (node 3) | [1] |
| 5 | Visit left of 3 (`nullptr`) | [1] |
| 6 | Add 3 | [1, 3] |
| 7 | Visit right of 3 (`nullptr`) | [1, 3] |
| 8 | Add 2 | [1, 3, 2] |
| 9 | Visit right of 2 (`nullptr`) | [1, 3, 2] |

Final output:

```cpp
[1, 3, 2]
```

## 5. Understand the Recursive Calls

Consider this tree:

```text
          1
         / \
        2   3
       / \
      4   5
```

Inorder traversal:

```text
4 → 2 → 5 → 1 → 3
```

Why?

- First, completely traverse the left subtree of `1`.
- Inside that subtree, visit `4`, then `2`, then `5`.
- Visit root `1`.
- Finally, visit right subtree `3`.

## 6. Complexity Analysis

Let `N` be the number of nodes.

- **Time Complexity:** \(O(N)\) — every node is visited exactly once.
- **Auxiliary Space:** \(O(H)\) — recursion stack, where `H` is the height of the tree.
- **Output Space:** \(O(N)\) — stores all node values.

For a balanced tree, recursion space is \(O(\log N)\). For a skewed tree, it is \(O(N)\).

## 7. Key Takeaways

1. Inorder = Left → Root → Right.
2. `root->left` accesses the left child.
3. `root->val` accesses the current node's value.
4. `root->right` accesses the right child.
5. The base case prevents accessing a null node.
6. In a Binary Search Tree (BST), inorder traversal produces values in sorted order.
