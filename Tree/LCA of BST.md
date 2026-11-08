# Lowest Common Ancestor of a Binary Search Tree — LeetCode 235

## Pattern
Binary Search Tree + DFS

## Is your code correct?
**Your approach is correct, but your code has a few edge cases that need fixing.**

### Issues in your code

**1. Missing NULL check**

```cpp
if(root == nullptr) return;
```

If the target node is not found or recursion reaches a NULL child, your code may access `root->val` and cause an error.

**2. Missing equality condition**

You currently check:

```cpp
root->val < q->val && root->val > p->val
```

But the LCA can also be equal to `p` or `q`. Your first condition handles equality only when the root pointer is exactly `p` or `q`.

A simpler condition is:

```cpp
if(root->val >= p->val && root->val <= q->val)
```

**3. Unnecessary `ans` and helper function**

You can directly return the LCA using recursion.

## Approach

Because this is a BST, we use its ordering property:

- Left subtree contains smaller values.
- Right subtree contains larger values.

First, make sure `p` has the smaller value and `q` has the larger value.

Then:

1. If `root->val < p->val`, both nodes are in the right subtree.
2. If `root->val > q->val`, both nodes are in the left subtree.
3. Otherwise, the current root is the LCA.

## Corrected Code (Recursive)

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {

        if(root == nullptr) return nullptr;

        if(p->val > q->val)
            swap(p, q);

        if(root->val < p->val) {
            return lowestCommonAncestor(root->right, p, q);
        }

        if(root->val > q->val) {
            return lowestCommonAncestor(root->left, p, q);
        }

        return root;
    }
};
```

## Dry Run

Consider this BST:

          6
         / \
        2   8
       / \ / \
      0  4 7  9
        / \
       3   5

Given:

```cpp
p = 2;
q = 8;
```

**Step 1: At root = 6**

```cpp
6 >= 2 && 6 <= 8
```

Both nodes lie on opposite sides of `6`.

Therefore, `6` is the LCA.

Output:

```cpp
6
```

### Another example

Given:

```cpp
p = 2;
q = 4;
```

At root `6`:

```cpp
6 > 4
```

Move left.

At root `2`:

```cpp
root == p
```

Return node `2`.

Output:

```cpp
2
```

This also demonstrates that a node can be its own ancestor.

## Complexity Analysis

Let H be the height of the BST.

- **Time Complexity:** O(H) — we visit only one path from root to the LCA.
- **Auxiliary Space:** O(H) — recursive call stack.

For a balanced BST: O(log N).

For a skewed BST: O(N).

## Iterative Alternative

You can also solve this without recursion:

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {

        while(root != nullptr) {

            if(root->val < p->val && root->val < q->val) {
                root = root->right;
            }
            else if(root->val > p->val && root->val > q->val) {
                root = root->left;
            }
            else {
                return root;
            }
        }

        return nullptr;
    }
};
```

## Binary Tree LCA vs BST LCA

| Binary Tree (LC 236) | BST (LC 235) |
|---|---|
| Uses DFS | Uses BST ordering |
| May explore both subtrees | Moves only left or right |
| O(N) time | O(H) time |
| Checks whether nodes exist in subtrees | Compares node values |

## Key Takeaways

- In a normal binary tree, we cannot decide the direction using values.
- In a BST, ordering helps us find the LCA efficiently.
- If both values are smaller, go left.
- If both values are larger, go right.
- Otherwise, the current node is the LCA.

**Remember:** BST LCA = Find the first node where the paths to `p` and `q` split, or where one of the nodes itself is reached.
