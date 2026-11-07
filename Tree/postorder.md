# Binary Tree Postorder Traversal — LeetCode 145

**Pattern:** Binary Tree + DFS + Recursion

## 1. Is your code correct?

<badge color="success">Correct</badge>

Your code is completely correct. You are implementing **recursive postorder traversal**.

Postorder traversal follows this order:

**Left → Right → Root**

## 2. Approach

For every node:

1. First, recursively traverse its left subtree.
2. Then, recursively traverse its right subtree.
3. Finally, store the current node's value.

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
    vector<int> postorderTraversal(TreeNode* root) {

        vector<int> ans;

        postorder(root, ans);

        return ans;
    }

    void postorder(TreeNode* root, vector<int>& ans) {

        if(root == nullptr) {
            return;
        }

        // 1. Left
        postorder(root->left, ans);

        // 2. Right
        postorder(root->right, ans);

        // 3. Root
        ans.push_back(root->val);
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
postorder(root, ans);
```

Execution:

| Step | Operation | ans |
|---|---|---|
| 1 | Visit left of 1 (`nullptr`) | [] |
| 2 | Visit right of 1 (node 2) | [] |
| 3 | Visit left of 2 (node 3) | [] |
| 4 | Visit left and right of 3 (`nullptr`) | [] |
| 5 | Add node 3 | [3] |
| 6 | Visit right of 2 (`nullptr`) | [3] |
| 7 | Add node 2 | [3, 2] |
| 8 | Visit right of 1 (`nullptr`) | [3, 2] |
| 9 | Add node 1 | [3, 2, 1] |

Final output:

```cpp
[3, 2, 1]
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

Postorder traversal:

```text
4 → 5 → 2 → 3 → 1
```

Why?

- First, completely process the left subtree of `1`: `4 → 5 → 2`.
- Then process the right subtree: `3`.
- Finally, process the root: `1`.

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

**Remember:** In postorder, the current node is processed only after both its left and right subtrees are completely traversed.

You have now covered all three basic recursive DFS traversals!
