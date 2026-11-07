# Subtree of Another Tree — LeetCode 572

## Pattern
Binary Tree + DFS + Recursion

## Is your code correct?
**Your approach is correct, but there is one edge case missing.**

If `subRoot == nullptr`, it should return `true`, because an empty tree is considered a subtree.

Add this condition at the beginning:

```cpp
if(subRoot == nullptr) return true;
```

## Approach

We use two recursive functions:

1. `isSubtree()` — searches for a node in the main tree from which the subtree might start.
2. `isSameTree()` — checks whether the tree starting at that node is exactly identical to `subRoot`.

### How it works

For every node in `root`:

1. Check whether the current subtree matches `subRoot` using `isSameTree()`.
2. If it matches, return `true`.
3. Otherwise, search in the left subtree.
4. Search in the right subtree.
5. If neither matches, return `false`.

## Corrected Code

```cpp
class Solution {
public:
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {

        if(subRoot == nullptr) return true;
        if(root == nullptr) return false;

        if(isSameTree(root, subRoot)) {
            return true;
        }

        return isSubtree(root->left, subRoot) ||
               isSubtree(root->right, subRoot);
    }

    bool isSameTree(TreeNode* root, TreeNode* subRoot) {

        if(root == nullptr && subRoot == nullptr)
            return true;

        if(root == nullptr || subRoot == nullptr)
            return false;

        if(root->val != subRoot->val)
            return false;

        return isSameTree(root->left, subRoot->left) &&
               isSameTree(root->right, subRoot->right);
    }
};
```

## Dry Run

Input:

```cpp
root    = [3,4,5,1,2]
subRoot = [4,1,2]
```

Main tree:

        3
       / \
      4   5
     / \
    1   2

Subtree:

        4
       / \
      1   2

**Step 1:** Start at node `3`.

```cpp
isSameTree(3, 4) // false
```

Values don't match.

**Step 2:** Move to the left child of `3`, which is `4`.

```cpp
isSameTree(4, 4)
```

Values match, so compare their children:

```cpp
isSameTree(1, 1) // true
isSameTree(2, 2) // true
```

Both left and right subtrees match.

Therefore:

```cpp
return true;
```

## Complexity Analysis

Let:
- N = number of nodes in `root`.
- M = number of nodes in `subRoot`.

**Time Complexity:** O(N × M) in the worst case.

- We may visit every node in `root`.
- At each node, `isSameTree()` may compare up to M nodes.

**Auxiliary Space:** O(H + K)

- H = height of the main tree.
- K = height of `subRoot`.
- These represent the recursive call stacks; the maximum simultaneous depth is bounded by O(H + K).

## Key Takeaways

- `isSubtree()` searches for a possible matching starting node.
- `isSameTree()` verifies the complete structure and values.
- Use `||` because a match in either the left or right subtree is sufficient.
- Use `&&` inside `isSameTree()` because both children must match.
- Handle `subRoot == nullptr` separately.

**Remember: Subtree = Search for a node + Check whether both trees are identical.**
