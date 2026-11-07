# Flip Equivalent Binary Trees — LeetCode 951

## Pattern
Binary Tree + DFS + Recursion + Backtracking-style choices

## Is your code correct?
**Yes, your code is completely correct.** ✅

You are checking two possibilities at every pair of corresponding nodes:

1. **Without flip:** Left matches left and right matches right.
2. **With flip:** Left matches right and right matches left.

If either possibility works, the trees are flip equivalent.

## Approach

### 1. Base Conditions

**Both nodes are NULL:**
```cpp
if(root1 == nullptr && root2 == nullptr) return true;
```
Both subtrees are empty, so they match.

**Only one node is NULL:**
```cpp
if(root1 == nullptr || root2 == nullptr) return false;
```
Their structures cannot match.

**Different node values:**
```cpp
if(root1->val != root2->val) return false;
```
A flip cannot change node values.

### 2. Without Flip

```cpp
bool withoutflip =
    flipEquiv(root1->left, root2->left) &&
    flipEquiv(root1->right, root2->right);
```

Compare corresponding children without swapping.

### 3. With Flip

```cpp
bool withflip =
    flipEquiv(root1->left, root2->right) &&
    flipEquiv(root1->right, root2->left);
```

Compare opposite children because their positions are swapped.

Finally:

```cpp
return withflip || withoutflip;
```

Either possibility is sufficient.

## Your Code

```cpp
class Solution {
public:
    bool flipEquiv(TreeNode* root1, TreeNode* root2) {
        if(root1 == nullptr && root2 == nullptr)
            return true;

        if(root1 == nullptr || root2 == nullptr)
            return false;

        if(root1->val != root2->val)
            return false;

        bool withoutflip =
            flipEquiv(root1->left, root2->left) &&
            flipEquiv(root1->right, root2->right);

        bool withflip =
            flipEquiv(root1->left, root2->right) &&
            flipEquiv(root1->right, root2->left);

        return withflip || withoutflip;
    }
};
```

## Dry Run

Consider these two trees:

Tree 1:
        1
       / \
      2   3

Tree 2:
        1
       / \
      3   2

**Step 1: Compare roots**

```cpp
root1->val == root2->val
1 == 1  // true
```

**Step 2: Check without flip**

```cpp
flipEquiv(2, 3) // false
```

Since values differ, `withoutflip` becomes false.

**Step 3: Check with flip**

```cpp
flipEquiv(2, 2) // true
flipEquiv(3, 3) // true
```

Therefore:

```cpp
withflip = true && true;
// true
```

**Step 4: Final result**

```cpp
return true || false;
// true
```

The trees are flip equivalent.

## Complexity Analysis

Let N be the number of nodes in the trees.

- **Time Complexity:** O(N²) worst case. In the worst case, both flip and non-flip possibilities can lead to repeated recursive comparisons.
- **Auxiliary Space:** O(H), where H is the maximum recursion depth.

## Key Takeaways

- Flip operation means swapping left and right subtrees.
- Node values must remain equal.
- At every pair of nodes, check both possibilities.
- Use `&&` because both child comparisons must succeed within one possibility.
- Use `||` because either flipped or unflipped arrangement is valid.

## Important Pattern to Remember

```cpp
// Without flip
left  with left
right with right

// With flip
left  with right
right with left
```

**Remember:** Flip Equivalent = Same Values + Same Structure after allowing child swaps.
