# Same Tree — LeetCode 100

## Pattern
Binary Tree + DFS + Recursion

## Is your code correct?
**Yes, your code is completely correct.** ✅

You are comparing both trees recursively, checking:
1. Whether their structures are identical.
2. Whether corresponding nodes have equal values.

## Approach

There are three important base conditions:

**1. Both nodes are NULL**
```cpp
if(p == nullptr && q == nullptr) return true;
```
Both trees have reached the end at the same position, so they match.

**2. Only one node is NULL**
```cpp
if(p == nullptr || q == nullptr) return false;
```
One tree has a node while the other does not. Their structures are different.

**3. Node values are different**
```cpp
if(p->val != q->val) return false;
```
Corresponding nodes must have equal values.

If all these conditions pass, recursively compare the left and right subtrees.

## Your Code

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(p == nullptr && q == nullptr) return true;

        if(p == nullptr || q == nullptr) return false;

        if(p->val != q->val) return false;

        return isSameTree(p->left, q->left) &&
               isSameTree(p->right, q->right);
    }
};
```

## Dry Run

Consider:

Tree P:
        1
       / \
      2   3

Tree Q:
        1
       / \
      2   3

| Comparison | Result |
|---|---|
| P root (1) == Q root (1) | True |
| P left (2) == Q left (2) | True |
| P right (3) == Q right (3) | True |
| Both corresponding NULL children | True |

Therefore:

```cpp
return true;
```

Now consider:

Tree P: `[1,2]`

Tree Q: `[1,null,2]`

The root values match, but their left and right child structures differ.

Therefore:

```cpp
return false;
```

## Complexity Analysis

Let N be the number of nodes in the smaller tree.

- **Time Complexity:** O(N) — each corresponding pair of nodes is compared at most once.
- **Auxiliary Space:** O(H) — recursion stack, where H is the maximum recursion depth.

## Key Takeaways

- Compare corresponding nodes, not just the values.
- Both NULL means the current subtrees match.
- One NULL means the structures differ.
- Use `&&` because both left and right subtrees must match.
- This is a classic example of recursive tree comparison.

**Remember:** Same Tree = Same Structure + Same Node Values.
