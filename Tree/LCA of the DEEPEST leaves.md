# Lowest Common Ancestor of Deepest Leaves — LeetCode 1123

## Pattern
Binary Tree + DFS + Postorder Traversal + Pair

## Is your code correct?
**Yes, your code is completely correct.** ✅

You are using an efficient bottom-up DFS approach.

At every node, your function returns two things:

```cpp
pair<int, TreeNode*>
```

- `first` → Height/depth information of the subtree.
- `second` → LCA of the deepest leaves in that subtree.

This allows you to calculate the answer in a single traversal.

## Approach

At every node, recursively calculate the results of its left and right subtrees.

There are three possibilities:

### Case 1: Both subtree heights are equal

```cpp
if(left.first == right.first) {
    return {left.first + 1, root};
}
```

The deepest leaves exist on both sides.

Therefore, the current node is their LCA.

### Case 2: Left subtree is deeper

```cpp
else if(left.first > right.first) {
    return {left.first + 1, left.second};
}
```

The deepest leaves are in the left subtree.

Therefore, the LCA must also be inside the left subtree.

### Case 3: Right subtree is deeper

```cpp
else {
    return {right.first + 1, right.second};
}
```

The deepest leaves are in the right subtree.

Therefore, return the LCA obtained from the right subtree.

## Your Code

```cpp
class Solution {
public:
    TreeNode* lcaDeepestLeaves(TreeNode* root) {
        if(root == nullptr) return root;

        auto ans = func(root);
        return ans.second;
    }

    pair<int, TreeNode*> func(TreeNode* root) {
        if(root == nullptr)
            return {0, nullptr};

        auto left = func(root->left);
        auto right = func(root->right);

        if(left.first == right.first) {
            return {left.first + 1, root};
        }
        else if(left.first > right.first) {
            return {left.first + 1, left.second};
        }
        else {
            return {right.first + 1, right.second};
        }
    }
};
```

**Small observation:** You declared a class-level `TreeNode* ans`, but you don't use it. You can remove it. Your local `auto ans` inside `lcaDeepestLeaves()` is sufficient.

## Dry Run

Consider this tree:

          3
         / \
        5   1
       / \ / \
      6  2 0  8
        / \
       7   4

The deepest leaves are `7` and `4`.

**Step 1: At node 7**

```cpp
left  = {0, nullptr}
right = {0, nullptr}
```

Heights are equal.

```cpp
return {1, node7};
```

**Step 2: At node 4**

Similarly:

```cpp
return {1, node4};
```

**Step 3: At node 2**

```cpp
left.first  = 1;
right.first = 1;
```

Both subtree heights are equal.

```cpp
return {2, node2};
```

So node `2` becomes the LCA of deepest leaves `7` and `4`.

**Step 4: At node 5**

- Left subtree height = 1 (node `6`).
- Right subtree height = 2 (subtree rooted at `2`).

Right subtree is deeper.

```cpp
return {3, node2};
```

**Step 5: At node 3**

- Left subtree height = 3.
- Right subtree height = 2.

Left subtree is deeper.

```cpp
return {4, node2};
```

Final answer:

```cpp
return node2;
```

## Complexity Analysis

Let N be the number of nodes and H be the height of the tree.

- **Time Complexity:** O(N) — every node is visited exactly once.
- **Auxiliary Space:** O(H) — recursive call stack.
  - Balanced tree: O(log N)
  - Skewed tree: O(N)

## Key Takeaways

- Use postorder traversal: Left → Right → Root.
- Calculate information from both children before processing the current node.
- Equal heights mean the current node is the LCA.
- Greater left height means the answer comes from the left subtree.
- Greater right height means the answer comes from the right subtree.
- `pair<int, TreeNode*>` stores both height and LCA together.

**Remember:** Deepest Leaves LCA = Compare subtree heights and move toward the deeper side. If both heights are equal, the current node is the answer.
