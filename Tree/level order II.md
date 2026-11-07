# Binary Tree Level Order Traversal II — LeetCode 107

## Pattern
BFS (Breadth-First Search) + Queue

## Is your code correct?
**Yes, your code is correct.** ✅

You are performing normal level-order traversal using BFS, but inserting each completed level at the beginning of `ans`.

```cpp
ans.insert(ans.begin(), level);
```

This makes the final result bottom-up.

## Approach

1. If `root == nullptr`, return an empty answer.
2. Push the root into the queue.
3. Process nodes level by level.
4. Store each level's values in a separate vector.
5. Insert that vector at the beginning of `ans`.
6. Return `ans`.

## Dry Run

Consider this tree:

       3
      / \
     9   20
        /  \
       15   7

**Queue and answer after each level:**

| Level processed | `level` | `ans` after insertion |
|---|---|---|
| 1 | `[3]` | `[[3]]` |
| 2 | `[9, 20]` | `[[9,20], [3]]` |
| 3 | `[15, 7]` | `[[15,7], [9,20], [3]]` |

Final output:

```cpp
[[15, 7], [9, 20], [3]]
```

## Alternative Approach

You can also use your normal level-order traversal and reverse the answer at the end.

```cpp
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> ans;

        if(root == nullptr) return ans;

        queue<TreeNode*> q;
        q.push(root);

        while(!q.empty()) {
            int size = q.size();
            vector<int> level;

            while(size--) {
                TreeNode* node = q.front();
                q.pop();

                level.push_back(node->val);

                if(node->left)
                    q.push(node->left);

                if(node->right)
                    q.push(node->right);
            }

            ans.push_back(level);
        }

        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

## Complexity Analysis

**Your approach:**

- Time: O(N × H) — inserting at the beginning shifts existing levels. Here, H is the number of levels.
- Auxiliary space: O(W) for the queue, where W is the maximum width of the tree.
- Output space: O(N).

**Alternative approach:**

- Time: O(N) — traversal plus reversing the levels.
- Auxiliary space: O(W) for the queue.
- Output space: O(N).

## Key Takeaways

- Normal level order traversal: `ans.push_back(level)`.
- Bottom-up traversal: `ans.insert(ans.begin(), level)` or reverse `ans` at the end.
- **Interview tip:** Prefer `push_back()` followed by `reverse()` because it avoids repeatedly shifting existing elements.
