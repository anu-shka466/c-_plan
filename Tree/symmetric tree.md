# Symmetric Tree — LeetCode 101

## Pattern
Binary Tree + DFS + Recursion

## Is your code correct?
**Yes, your code is completely correct.** ✅

You are using recursive DFS to check whether the left and right subtrees are mirror images of each other.

The important difference from **Same Tree** is that here we compare opposite children.

## Approach

1. If the root is `nullptr`, the tree is symmetric.
2. Compare the root's left and right subtrees.
3. Check the following conditions:

**1. Both nodes are NULL**
```cpp
if(left == nullptr && right == nullptr) return true;
```
Both sides have ended, so they are symmetric.

**2. Only one node is NULL**
```cpp
if(left == nullptr || right == nullptr) return false;
```
The structures are different.

**3. Node values are different**
```cpp
if(left->val != right->val) return false;
```
Corresponding mirror nodes must have equal values.

**4. Recursive comparison**
```cpp
return symmetry(left->left, right->right) &&
       symmetry(left->right, right->left);
```

Notice that we compare:
- Left subtree's left child with right subtree's right child.
- Left subtree's right child with right subtree's left child.

## Your Code

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr) return true;

        return symmetry(root->left, root->right);
    }

    bool symmetry(TreeNode* left, TreeNode* right) {
        if(left == nullptr && right == nullptr) return true;

        if(left == nullptr || right == nullptr) return false;

        if(left->val != right->val) return false;

        return symmetry(left->left, right->right) &&
               symmetry(left->right, right->left);
    }
};
```

## Dry Run

Consider this symmetric tree:

          1
         / \
        2   2
       / \ / \
      3  4 4  3

**Step 1:** Compare root's left and right children.

```cpp
symmetry(2, 2)  // true so far
```

**Step 2:** Compare outer children.

```cpp
symmetry(3, 3)  // true
```

**Step 3:** Compare inner children.

```cpp
symmetry(4, 4)  // true
```

**Step 4:** Both corresponding subtrees match, so the function returns `true`.

Final output:

```cpp
true
```

## Complexity Analysis

Let N be the number of nodes and H be the height of the tree.

- **Time Complexity:** O(N) — each node is visited at most once.
- **Auxiliary Space:** O(H) — recursive call stack.
  - Balanced tree: O(log N)
  - Skewed tree: O(N)

## Same Tree vs Symmetric Tree

| Same Tree | Symmetric Tree |
|---|---|
| Compare left with left | Compare left with right |
| Compare right with right | Compare right with left |
| Checks whether two trees are identical | Checks whether one tree is a mirror of itself |

## Key Takeaway

**Symmetric Tree = Mirror Comparison**

Remember this recursive pattern:

```cpp
symmetry(left->left, right->right) &&
symmetry(left->right, right->left);
```
