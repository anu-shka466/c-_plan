# Binary Tree Zigzag Level Order Traversal — LeetCode 103

**Pattern:** Binary Tree + BFS + Queue

## 1. Is your code correct?

<badge color="success">Correct</badge>

Your code is completely correct. You are using **BFS with a queue**, just like normal level order traversal.

The additional logic is:

```cpp
bool lefttoright = 0;
```

You use this variable to alternate the direction of traversal at every level.

- `false (0)` → Left to Right
- `true (1)` → Right to Left

Your approach collects each level normally and reverses alternate levels.

## 2. Approach

1. If root is `nullptr`, return an empty answer.
2. Push the root into the queue.
3. Process nodes level by level using `size = q.size()`.
4. Store the current level's values in a temporary vector.
5. If `lefttoright == false`, insert the level directly.
6. Otherwise, reverse the level before inserting it.
7. Toggle the direction after every level.

The important operations are:

```cpp
reverse(level.begin(), level.end());
lefttoright = !lefttoright;
```

## 3. Code

Your implementation is correct:

```cpp
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {

        vector<vector<int>> ans;

        if(root == nullptr) return ans;

        queue<TreeNode*> q;
        q.push(root);

        bool lefttoright = 0;

        while(!q.empty()) {

            int size = q.size();
            vector<int> level;

            while(size--) {

                TreeNode* newnode = q.front();
                q.pop();

                level.push_back(newnode->val);

                if(newnode->left != nullptr) {
                    q.push(newnode->left);
                }

                if(newnode->right != nullptr) {
                    q.push(newnode->right);
                }
            }

            if(lefttoright == 0) {
                ans.push_back(level);
            }
            else {
                reverse(level.begin(), level.end());
                ans.push_back(level);
            }

            lefttoright = !lefttoright;
        }

        return ans;
    }
};
```

## 4. Dry Run

Consider this binary tree:

```text
              1
             / \
            2   3
           / \ / \
          4  5 6  7
```

Initially:

```cpp
queue = [1]
lefttoright = false
ans = []
```

**Level 1:**

```text
size = 1
level = [1]
```

Direction: Left to Right.

```text
ans = [[1]]
lefttoright = true
queue = [2, 3]
```

**Level 2:**

Queue contains:

```text
[2, 3]
```

Process both nodes and collect:

```text
level = [2, 3]
```

Direction: Right to Left.

Reverse:

```cpp
reverse(level.begin(), level.end());
```

Now:

```text
level = [3, 2]
ans = [[1], [3, 2]]
lefttoright = false
queue = [4, 5, 6, 7]
```

**Level 3:**

Collect:

```text
level = [4, 5, 6, 7]
```

Direction: Left to Right.

No reversal is required.

```text
ans = [[1], [3, 2], [4, 5, 6, 7]]
```

Final output:

```cpp
[[1], [3, 2], [4, 5, 6, 7]]
```

## 5. Why do we use `lefttoright = !lefttoright`?

This statement:

```cpp
lefttoright = !lefttoright;
```

toggles the Boolean value.

| Current value | After toggling |
|---|---|
| false | true |
| true | false |
| false | true |
| true | false |

Therefore, the traversal direction changes after every level automatically.

## 6. Important Observation

Notice that we always insert the left child first and then the right child:

```cpp
if(newnode->left != nullptr) {
    q.push(newnode->left);
}

if(newnode->right != nullptr) {
    q.push(newnode->right);
}
```

**Why?**

Because the queue maintains the correct BFS processing order.

We only reverse the collected `level` vector. We do not need to change how nodes are inserted into the queue.

This keeps the implementation simple.

## 7. Complexity Analysis

Let `N` be the number of nodes and `W` be the maximum width of the tree.

- **Time Complexity:** \(O(N)\) — every node is processed once. Reversing all levels takes at most O(N) additional work overall.
- **Auxiliary Space:** \(O(W)\) — queue storage.
- **Output Space:** \(O(N)\) — stores all node values.

## 8. Key Takeaways

1. Zigzag traversal is a variation of level order traversal.
2. It uses BFS and a queue.
3. `size = q.size()` separates levels.
4. A Boolean variable controls the direction.
5. `reverse()` reverses alternate levels.
6. `!lefttoright` toggles the direction.
7. We always enqueue left child first and right child second.

**Interview tip:** Your solution is easy to explain and has optimal \(O(N)\) time complexity.
