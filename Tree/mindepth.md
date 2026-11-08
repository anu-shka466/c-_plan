# Minimum Depth of Binary Tree — LeetCode 111

**Pattern:** Binary Tree + DFS (Recursion)

## 1. Is your code correct?

<badge color="success">Your code is correct</badge>

Your logic is absolutely correct. You have handled an important edge case: **a node with only one child must not be treated as a leaf.**

## 2. Understanding your approach

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root == nullptr) return 0;

        int left = minDepth(root->left);
        int right = minDepth(root->right);

        if(left == 0) return right + 1;
        if(right == 0) return left + 1;

        return min(left, right) + 1;
    }
};
```

Let's understand each condition.

**Base case:**
```cpp
if(root == nullptr) return 0;
```
An empty tree has depth 0.

**Recursive calls:**
```cpp
int left = minDepth(root->left);
int right = minDepth(root->right);
```
Calculate the minimum depth of both subtrees.

**Condition 1:**
```cpp
if(left == 0) return right + 1;
```
If the left child is absent, we must go through the right subtree.

**Condition 2:**
```cpp
if(right == 0) return left + 1;
```
If the right child is absent, we must go through the left subtree.

**Condition 3:**
```cpp
return min(left, right) + 1;
```
If both children exist, choose the smaller depth and add the current node.

## 3. Dry run

Consider this tree:

```text
       1
      /
     2
    /
   3
```

| Node | Left depth | Right depth | Returned depth |
|---|---:|---:|---:|
| 3 | 0 | 0 | 1 |
| 2 | 1 | 0 | 2 |
| 1 | 2 | 0 | 3 |

<badge color="success">Answer = 3</badge>

Notice that the algorithm doesn't consider the missing right child of node 1 as a leaf. The nearest actual leaf is node 3.

## 4. Complexity analysis

- **Time Complexity:** O(N), because every node is visited once.
- **Auxiliary Space:** O(H), due to the recursion stack, where H is the tree height.

## 5. Important interview concept

For **minimum depth**, remember:

- If both children exist → take `min(left, right) + 1`.
- If one child is missing → take the other child's depth + 1.
- If the node is null → return 0.

This missing-child condition is the most important difference between minimum-depth and maximum-depth recursion.
