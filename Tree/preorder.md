# Binary Tree Preorder Traversal — LeetCode 144

**Pattern:** Binary Tree + DFS + Recursion

## 1. Is your code correct?

<badge color="success">Correct</badge>

Your code is completely correct. You are implementing **recursive preorder traversal**.

Preorder traversal follows this order:

**Root → Left → Right**

## 2. Approach

For every node:

1. First, visit and store the current node's value.
2. Recursively traverse its left subtree.
3. Recursively traverse its right subtree.

**Base case:**

```cpp
if(root == nullptr) {
    return;
}
```

If the current node is `nullptr`, stop the recursive call.

## 3. Code

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {

        vector<int> ans;

        preorder(root, ans);

        return ans;
    }

    void preorder(TreeNode* root, vector<int>& ans) {

        if(root == nullptr) {
            return;
        }

        // 1. Root
        ans.push_back(root->val);

        // 2. Left
        preorder(root->left, ans);

        // 3. Right
        preorder(root->right, ans);
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
preorder(root, ans);
```

Execution:

| Step | Operation | ans |
|---|---|---|
| 1 | Add node 1 | [1] |
| 2 | Visit left of 1 (`nullptr`) | [1] |
| 3 | Visit right of 1 (node 2) | [1] |
| 4 | Add node 2 | [1, 2] |
| 5 | Visit left of 2 (node 3) | [1, 2] |
| 6 | Add node 3 | [1, 2, 3] |
| 7 | Visit left and right of 3 (`nullptr`) | [1, 2, 3] |
| 8 | Visit right of 2 (`nullptr`) | [1, 2, 3] |

Final output:

```cpp
[1, 2, 3]
```

## 5. Understand the Traversal Order

Consider this tree:

```text
          1
         / \
        2   3
       / \
      4   5
```

Preorder traversal:

```text
1 → 2 → 4 → 5 → 3
```

Why?

- First visit root `1`.
- Traverse the entire left subtree: `2 → 4 → 5`.
- Finally traverse the right subtree: `3`.

## 6. Complexity Analysis

Let `N` be the number of nodes and `H` be the height of the tree.

- **Time Complexity:** \(O(N)\) — every node is visited exactly once.
- **Auxiliary Space:** \(O(H)\) — recursion stack.
- **Output Space:** \(O(N)\) — stores all node values.

For a balanced tree, recursion space is \(O(\log N)\). For a skewed tree, it is \(O(N)\).

## 7. Key Takeaways

| Traversal | Order |
|---|---|
| Preorder | Root → Left → Right |
| Inorder | Left → Root → Right |
| Postorder | Left → Right → Root |

**Remember:** In preorder, we process the current node before visiting its children.
