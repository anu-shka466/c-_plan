# Binary Tree Level Order Traversal — LeetCode 102

**Pattern:** Binary Tree + BFS (Breadth-First Search) + Queue

## 1. Is your code correct?

<badge color="success">Correct</badge>

Your code is completely correct. You are using **BFS with a queue** to traverse the tree level by level.

Unlike preorder, inorder, and postorder, which commonly use DFS, level order traversal uses BFS.

## 2. Approach

We use a queue because it follows **FIFO (First In, First Out)**.

Steps:

1. If the root is `nullptr`, return an empty answer.
2. Push the root into the queue.
3. Run a loop while the queue is not empty.
4. Store the current queue size in `size`. This represents the number of nodes at the current level.
5. Process exactly `size` nodes:
   - Remove the front node.
   - Add its value to the current level.
   - Push its left child if it exists.
   - Push its right child if it exists.
6. After processing the entire level, add that level to `ans`.

## 3. Code

Your implementation is correct:

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {

        vector<vector<int>> ans;

        if(root == nullptr) return ans;

        queue<TreeNode*> q;
        q.push(root);

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

            ans.push_back(level);
        }

        return ans;
    }
};
```

## 4. Dry Run

Consider this tree:

```text
              1
             / \
            2   3
           / \   \
          4   5   6
```

Initially:

```cpp
queue = [1]
ans = []
```

**Level 1:**

```text
size = 1

Remove 1
Add 1 to level
Push 2 and 3 into queue
```

After processing:

```text
level = [1]
queue = [2, 3]
ans = [[1]]
```

**Level 2:**

```text
size = 2

Remove 2 → Push 4, 5
Remove 3 → Push 6
```

After processing:

```text
level = [2, 3]
queue = [4, 5, 6]
ans = [[1], [2, 3]]
```

**Level 3:**

```text
size = 3

Remove 4
Remove 5
Remove 6
```

After processing:

```text
level = [4, 5, 6]
queue = []
ans = [[1], [2, 3], [4, 5, 6]]
```

Final output:

```cpp
[[1], [2, 3], [4, 5, 6]]
```

## 5. Why do we store `int size = q.size()`?

This is the most important part of level order traversal.

Suppose the queue is:

```text
[2, 3]
```

Both nodes belong to the current level.

We store:

```cpp
int size = q.size();  // 2
```

Now we process exactly two nodes.

While processing node `2`, its children `4` and `5` are pushed into the queue.

While processing node `3`, its child `6` is pushed.

The queue becomes:

```text
[4, 5, 6]
```

But these newly inserted nodes belong to the **next level**.

Because we stored the original size (`2`), the inner loop processes only the current level.

Without this size variable, we could accidentally process nodes from multiple levels together.

## 6. Complexity Analysis

Let `N` be the total number of nodes and `W` be the maximum width of the tree.

- **Time Complexity:** \(O(N)\) — every node is inserted into and removed from the queue exactly once.
- **Auxiliary Space:** \(O(W)\) — the queue stores nodes of the current or next level.
- **Output Space:** \(O(N)\) — stores all node values.

In the worst case, queue space can be \(O(N)\).

## 7. DFS vs BFS

| DFS | BFS |
|---|---|
| Goes deep before exploring other branches | Explores level by level |
| Uses recursion or stack | Uses queue |
| Preorder, Inorder, Postorder | Level Order |
| Depth-first | Breadth-first |

## 8. Key Takeaways

1. Level order traversal uses BFS.
2. BFS uses a queue (FIFO).
3. `q.push(root)` starts the traversal.
4. `q.front()` accesses the first node.
5. `q.pop()` removes the processed node.
6. `size = q.size()` separates levels.
7. Push left child first, then right child, to maintain left-to-right order.
