# Maximum Depth of Binary Tree — LeetCode 104

**Pattern:** Binary Tree + DFS (Recursion)

## 1. Approach

We use recursion to calculate the height of the left and right subtrees.

Steps:
1. If the current node is `nullptr`, return `0`.
2. Calculate the maximum depth of the left subtree.
3. Calculate the maximum depth of the right subtree.
4. Return the larger depth + 1 for the current node.

## 2. C++ Code

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;

        int left = maxDepth(root->left);
        int right = maxDepth(root->right);

        return max(left, right) + 1;
    }
};
```

## 3. Dry run

Consider:

```text
        1
       / \
      2   3
     /
    4
   /
  5
```

| Node | Left depth | Right depth | Returned depth |
|---|---:|---:|---:|
| 5 | 0 | 0 | 1 |
| 4 | 1 | 0 | 2 |
| 2 | 2 | 0 | 3 |
| 3 | 0 | 0 | 1 |
| 1 | 3 | 1 | 4 |

<badge color="success">Answer = 4</badge>

## 4. Complexity analysis

- **Time Complexity:** O(N), every node is visited once.
- **Auxiliary Space:** O(H), due to the recursion stack, where H is the height of the tree.

## 5. Minimum Depth vs Maximum Depth

| Minimum Depth | Maximum Depth |
|---|---|
| Shortest path to a leaf | Longest path to a leaf |
| Uses `min(left, right)` | Uses `max(left, right)` |
| Must handle a missing child separately | No special missing-child condition needed |

**Remember:** Maximum depth is simply `max(left, right) + 1`.
