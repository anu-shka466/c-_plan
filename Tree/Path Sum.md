# Path Sum — LeetCode 112

## Pattern
Binary Tree + DFS + Recursion

## Is your code correct?
**Yes, your code is completely correct.** ✅

You are using DFS to check whether at least one root-to-leaf path has a sum equal to `targetSum`.

Your approach is efficient because you subtract the current node's value from the target as you move down the tree.

## Approach

1. If `root == nullptr`, return `false`.
2. Subtract the current node's value from `targetSum`.
3. Check whether the current node is a leaf.
4. If it is a leaf and the remaining target is `0`, return `true`.
5. Otherwise, recursively search the left and right subtrees.
6. If either subtree has a valid path, return `true`.

## Your Code

```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root == nullptr) return false;

        targetSum -= root->val;

        if(root->left == nullptr &&
           root->right == nullptr &&
           targetSum == 0) {
            return true;
        }

        return hasPathSum(root->left, targetSum) ||
               hasPathSum(root->right, targetSum);
    }
};
```

## Dry Run

Consider this tree:

          5
         / \
        4   8
       /   / \
      11  13  4
     /  \       \
    7    2       1

Given:

```cpp
targetSum = 22;
```

**Step 1: Start at root (5)**

```cpp
targetSum = 22 - 5 = 17;
```

Move to the left child.

**Step 2: At node (4)**

```cpp
targetSum = 17 - 4 = 13;
```

Move to node `11`.

**Step 3: At node (11)**

```cpp
targetSum = 13 - 11 = 2;
```

Explore its left child `7`.

**Step 4: At node (7)**

```cpp
targetSum = 2 - 7 = -5;
```

Node `7` is a leaf, but the remaining target is not `0`.

Return `false`.

**Step 5: Backtrack and explore node (2)**

At node `2`:

```cpp
targetSum = 2 - 2 = 0;
```

Node `2` is a leaf and the remaining target is `0`.

Return `true`.

Therefore, the valid path is:

```cpp
5 -> 4 -> 11 -> 2
```

Sum:

```cpp
5 + 4 + 11 + 2 = 22
```

Final output:

```cpp
true
```

## Why do we check the leaf condition?

```cpp
root->left == nullptr && root->right == nullptr
```

The question specifically asks for a **root-to-leaf path**.

Reaching the target sum at an intermediate node is not enough. We must reach a leaf as well.

## Complexity Analysis

Let N be the number of nodes and H be the height of the tree.

- **Time Complexity:** O(N) — in the worst case, every node is visited.
- **Auxiliary Space:** O(H) — recursive call stack.
  - Balanced tree: O(log N)
  - Skewed tree: O(N)

## Key Takeaways

- Use DFS to explore root-to-leaf paths.
- Subtract the current node's value from the target.
- Check the remaining target only at a leaf.
- Use `||` because finding one valid path is sufficient.
- Negative remaining sums do not automatically mean failure because tree nodes may contain negative values.

**Remember:** Path Sum = Reach a leaf with the remaining target equal to zero.
