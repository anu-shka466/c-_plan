# Job Sequencing Problem — Greedy + Disjoint Set Union (DSU)

## Pattern
- Greedy
- Sorting
- Disjoint Set Union (DSU)
- Path Compression

## Approach

We need to maximize profit while completing as many profitable jobs as possible within their deadlines.

**Greedy idea:** Always consider the job with the highest profit first and assign it to the latest available slot before its deadline.

Why the latest slot?

Because placing a job as late as possible leaves earlier slots available for jobs with tighter deadlines.

### Steps

1. Create pairs `{profit, deadline}` for every job.
2. Sort jobs in descending order of profit.
3. Find the maximum deadline.
4. Create a `parent` array where each index represents a time slot.
   - Initially, every slot is its own parent.
   - `parent[i] = i` means slot `i` is available.
5. For every job:
   - Find the latest available slot using `find(deadline, parent)`.
   - If `slot > 0`, schedule the job.
   - Add its profit and increment the job count.
   - Mark that slot as occupied by connecting it to the previous available slot:
     `parent[slot] = find(slot - 1, parent)`.
6. Return `{total, maxprofit}`.

## Code (C++)

```cpp
class Solution {
public:

    int find(int x, vector<int>& parent) {
        if (parent[x] == x) return x;

        return parent[x] = find(parent[x], parent);
    }

    vector<int> jobSequencing(vector<int>& deadline, vector<int>& profit) {

        int n = profit.size();

        vector<pair<int, int>> jobs;

        for (int i = 0; i < n; i++) {
            jobs.push_back({profit[i], deadline[i]});
        }

        // Sort jobs by descending profit
        sort(jobs.rbegin(), jobs.rend());

        int maxdeadline = *max_element(deadline.begin(), deadline.end());

        vector<int> parent(maxdeadline + 1);

        for (int i = 0; i <= maxdeadline; i++) {
            parent[i] = i;
        }

        int total = 0;
        int maxprofit = 0;

        for (auto job : jobs) {

            int p = job.first;
            int d = job.second;

            // Find latest available slot
            int slot = find(d, parent);

            if (slot > 0) {

                total++;
                maxprofit += p;

                // Mark this slot as occupied
                parent[slot] = find(slot - 1, parent);
            }
        }

        return {total, maxprofit};
    }
};
```

## Dry Run

Input:

```cpp
deadline = {4, 1, 1, 1};
profit   = {20, 10, 40, 30};
```

### Step 1: Create jobs

```cpp
{20, 4}
{10, 1}
{40, 1}
{30, 1}
```

After sorting by descending profit:

```cpp
{40, 1}
{30, 1}
{20, 4}
{10, 1}
```

### Step 2: Initialize parent array

Maximum deadline = 4.

```cpp
parent = {0, 1, 2, 3, 4};
```

Each slot is initially available.

### Step 3: Schedule jobs

| Job | Profit | Deadline | Available Slot | Decision |
|---|---:|---:|---:|---|
| 1 | 40 | 1 | 1 | Schedule |
| 2 | 30 | 1 | 0 | Reject |
| 3 | 20 | 4 | 4 | Schedule |
| 4 | 10 | 1 | 0 | Reject |

Final schedule:

| Time Slot | Job | Profit |
|---|---|---:|
| 1 | Job with profit 40 | 40 |
| 4 | Job with profit 20 | 20 |

Final answer:

```cpp
{2, 60}
```

## Understanding DSU in This Problem

DSU normally helps us find and manage connected components.

Here, we use it differently: **to quickly find the latest available time slot.**

### 1. What does `find()` do?

```cpp
int find(int x, vector<int>& parent) {
    if (parent[x] == x) return x;

    return parent[x] = find(parent[x], parent);
}
```

It returns the latest available slot at or before `x`.

Path compression makes future searches faster.

### 2. What happens after scheduling a job?

Suppose slot 3 is occupied.

```cpp
parent[3] = find(2, parent);
```

Now slot 3 points to the latest available slot before it.

For example:

```cpp
parent[3] = 2;
```

If slot 2 is also occupied later, `find(3)` will eventually return the next available slot before 2.

### 3. Why do we check `slot > 0`?

Slot 0 represents that no valid slot is available.

Since jobs take one unit of time, we use slots:

```text
1, 2, 3, 4, ...
```

Therefore, a job can only be scheduled if `slot > 0`.

## Complexity

Let:
- `n` = number of jobs.
- `D` = maximum deadline.

| Operation | Complexity |
|---|---|
| Creating jobs | O(n) |
| Sorting | O(n log n) |
| Initializing parent | O(D) |
| DSU operations | Nearly O(n α(D)) |

**Total Time:** O(n log n + D)

**Space:** O(n + D)

Here, α represents the inverse Ackermann function, which grows extremely slowly.

## Key Takeaways

- Sort jobs by descending profit.
- Always try to schedule a job at the latest available slot before its deadline.
- DSU helps find the latest available slot efficiently.
- `find()` uses path compression.
- `parent[slot] = find(slot - 1, parent)` marks a slot as occupied.
- Your approach is correct and more efficient than checking every slot manually.

**Interview tip:** The important observation is that after a slot is occupied, we never need to search that slot again. DSU lets us skip occupied slots efficiently.
