# Invert Binary Tree — LeetCode 226

## Pattern
Binary Tree + DFS + Recursion

## Is your code correct?
**Yes, your code is completely correct.** ✅

You are using a recursive DFS approach. At every node, you swap its left and right children, then recursively invert both subtrees.

## Approach

1. Base case: If `root == nullptr`, return `root`.
2. Swap the left and right child pointers.
3. Recursively invert the left subtree.
4. Recursively invert the right subtree.
5. Return the root.

## Your Code

```cpp
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr) return root;

        TreeNode* temp = root->left;
        root->left = root->right;
        root->right = temp;

        invertTree(root->left);
        invertTree(root->right);

        return root;
    }
};
```

## Dry Run

Input tree:

        4
       / \
      2   7
     / \ / \
    1  3 6  9

**Step 1: At node 4**

Swap its children: `2` and `7`.

        4
       / \
      7   2

**Step 2: At node 7**

Swap its children: `6` and `9`.

        4
       / \
      7   2
     / \
    9   6

**Step 3: At node 2**

Swap its children: `1` and `3`.

        4
       / \
      7   2
     / \ / \
    9  6 3  1

**Step 4:** Both subtrees are processed. The recursion reaches `nullptr` and returns.

Final output:

```cpp
[4, 7, 2, 9, 6, 3, 1]
```

## Complexity Analysis

Let N be the number of nodes and H be the height of the tree.

- **Time Complexity:** O(N) — every node is visited exactly once.
- **Auxiliary Space:** O(H) — recursive call stack.
  - Balanced tree: O(log N)
  - Skewed tree: O(N)

## Key Takeaways

- `temp` temporarily stores the left child so that it is not lost during swapping.
- Swap children **before** making recursive calls.
- `invertTree(root->left)` processes the left subtree.
- `invertTree(root->right)` processes the right subtree.
- This modifies the original tree; it does not create a new tree.

**Remember:** Invert Binary Tree = Swap Left and Right at Every Node.
