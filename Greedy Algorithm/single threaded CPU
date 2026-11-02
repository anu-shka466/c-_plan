# Single-Threaded CPU — LeetCode 1834

## Pattern
- Sorting + Min Heap (Priority Queue)
- Greedy Algorithm

## Approach

We need to select the available task with:
1. Shortest processing time.
2. Smallest original index if processing times are equal.

A min heap helps us efficiently select the next task.

### Steps

1. Add the original index to every task:
   `tasks[i] = {enqueueTime, processingTime, index}`

2. Sort tasks by enqueue time.

3. Initialize:
   - `timer` = first task's enqueue time.
   - `i` = 0, to track tasks not yet added to the heap.
   - Min heap storing `{processingTime, index}`.

4. Add all tasks whose enqueue time is `<= timer` to the heap.

5. If the heap is empty:
   - No task is currently available.
   - Move `timer` to the next task's enqueue time.

6. Otherwise:
   - Pick the heap's top task.
   - Add its index to the answer.
   - Increase `timer` by its processing time.
   - Remove it from the heap.

7. Repeat until all tasks are processed.

## Code (C++)

```cpp
class Solution {
public:
    vector<int> getOrder(vector<vector<int>>& tasks) {
        vector<int> ans;
        int n = tasks.size();

        for (int i = 0; i < n; i++) {
            tasks[i].push_back(i);
        }

        sort(tasks.begin(), tasks.end());

        long long timer = tasks[0][0];
        int i = 0;

        priority_queue<
            pair<int, int>,
            vector<pair<int, int>>,
            greater<pair<int, int>>
        > pq;

        while (i < n || !pq.empty()) {

            // Add all currently available tasks
            while (i < n && tasks[i][0] <= timer) {
                pq.push({tasks[i][1], tasks[i][2]});
                i++;
            }

            if (pq.empty()) {
                // CPU is idle: jump to next task's arrival time
                timer = tasks[i][0];
            } 
            else {
                // Process the task with minimum processing time
                ans.push_back(pq.top().second);

                timer += pq.top().first;

                pq.pop();
            }
        }

        return ans;
    }
};
```

## Dry Run

Consider:

```cpp
tasks = {{1, 2}, {2, 4}, {3, 2}, {4, 1}}
```

After adding original indices and sorting:

```cpp
{1, 2, 0}
{2, 4, 1}
{3, 2, 2}
{4, 1, 3}
```

| Timer | Available tasks | Heap (processingTime, index) | Selected |
|---|---|---|---|
| 1 | 0 | (2,0) | 0 |
| 3 | 1, 2 | (2,2), (4,1) | 2 |
| 5 | 1, 3 | (1,3), (4,1) | 3 |
| 6 | 1 | (4,1) | 1 |

Final answer:

```cpp
[0, 2, 3, 1]
```

## Understanding the Min Heap

```cpp
priority_queue<
    pair<int, int>,
    vector<pair<int, int>>,
    greater<pair<int, int>>
> pq;
```

This creates a **min heap of pairs**.

For pairs, C++ compares:
1. First element.
2. If first elements are equal, second element.

Therefore:

```cpp
pq.push({2, 5});
pq.push({2, 3});
pq.push({1, 7});
```

The top will be:

```cpp
{1, 7}
```

After removing it, the top will be:

```cpp
{2, 3}
```

This automatically implements both conditions:
- Minimum processing time.
- Minimum index when processing times are equal.

## Complexity

Let `n` be the number of tasks.

- **Sorting:** O(n log n)
- **Heap insertion:** O(n log n)
- **Heap removal:** O(n log n)
- **Total Time:** O(n log n)
- **Space:** O(n) for the heap and answer.

## Key Takeaways

- Sorting helps us process tasks in enqueue-time order.
- Min heap helps us select the shortest available task.
- `pair<int,int>` automatically handles the index tie-breaker.
- `long long timer` prevents integer overflow.
- If the heap is empty, jump the timer to the next enqueue time instead of incrementing it one by one.
- Your approach is **correct and optimal**.

**Important edge case:** Your code assumes `n > 0`, which is guaranteed by LeetCode's constraints for this problem.
