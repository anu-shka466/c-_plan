# Task Scheduler — LeetCode 621

## Pattern
Greedy + Frequency Counting + Mathematics

## Approach

The key idea is that the task with the **maximum frequency** determines the minimum number of CPU intervals required.

We use a frequency array of size 26 because tasks are labeled from A to Z.

### Step 1: Count the frequency of each task

```cpp
vector<int> freq(26, 0);
int count = 0;

for(int i = 0; i < tasks.size(); i++) {
    freq[tasks[i] - 'A']++;
    count = max(count, freq[tasks[i] - 'A']);
}
```

- `freq[i]` stores the frequency of a task.
- `count` stores the maximum frequency among all tasks.

### Step 2: Calculate the minimum intervals

```cpp
int ans = (count - 1) * (n + 1);
```

Why?

Suppose:

```cpp
tasks = {'A','A','A','B','B','B'};
n = 2;
```

The maximum frequency is 3.

We arrange the most frequent task first:

```text
A _ _ | A _ _ | A
```

There are `count - 1` gaps between consecutive occurrences of A.

Each gap has:

- 1 position for task A.
- `n` positions for other tasks or idle intervals.

Therefore:

\[
(count-1)\times(n+1)
\]

For our example:

\[
(3-1)\times(2+1)=6
\]

### Step 3: Handle tasks having the same maximum frequency

```cpp
for(int i = 0; i < 26; i++) {
    if(freq[i] == count) {
        ans++;
    }
}
```

Suppose:

```text
A A A
B B B
```

Both A and B have the maximum frequency of 3.

We need to accommodate both in the final group:

```text
A _ _ | A _ _ | A
B _ _ | B _ _ | B
```

So we add the number of tasks having the maximum frequency.

For our example:

\[
ans = 6 + 2 = 8
\]

### Step 4: Compare with the total number of tasks

```cpp
return max(ans, (int)tasks.size());
```

Why?

The calculated arrangement might contain idle intervals.

But if we have enough different tasks to fill all idle positions, no idle time is needed.

For example:

```cpp
tasks = {'A','A','A','B','B','B','C','D','E'};
n = 2;
```

The formula gives 8, but there are 9 tasks.

We cannot complete 9 tasks in fewer than 9 intervals.

Therefore, return the maximum of:
- Calculated intervals.
- Total number of tasks.

## Code (C++)

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {

        vector<int> freq(26, 0);
        int count = 0;

        // Count frequencies and find maximum frequency
        for(int i = 0; i < tasks.size(); i++) {
            freq[tasks[i] - 'A']++;
            count = max(count, freq[tasks[i] - 'A']);
        }

        // Calculate intervals based on maximum frequency
        int ans = (count - 1) * (n + 1);

        // Add tasks having maximum frequency
        for(int i = 0; i < 26; i++) {
            if(freq[i] == count) {
                ans++;
            }
        }

        return max(ans, (int)tasks.size());
    }
};
```

## Dry Run

**Input:**

```cpp
tasks = {'A','A','A','B','B','B'};
n = 2;
```

### Frequency array

| Task | Frequency |
|---|---:|
| A | 3 |
| B | 3 |

Maximum frequency:

```cpp
count = 3;
```

### Calculate the answer

```cpp
ans = (count - 1) * (n + 1);
ans = (3 - 1) * (2 + 1);
ans = 6;
```

Both A and B have frequency 3.

```cpp
ans = 6 + 2;
ans = 8;
```

Total tasks = 6.

```cpp
return max(8, 6);
```

**Output:**

```cpp
8
```

One valid schedule:

```text
A → B → idle → A → B → idle → A → B
```

## Complexity Analysis

- **Time Complexity:** `O(N + 26)` = `O(N)`, where N is the number of tasks.
- **Space Complexity:** `O(26)` = `O(1)`, because the frequency array always has 26 elements.

## Key Takeaways

- The maximum task frequency determines the initial schedule structure.
- `(count - 1) * (n + 1)` creates the required gaps between the most frequent task occurrences.
- Add the number of tasks having the maximum frequency.
- Finally, take `max(ans, tasks.size())` because idle intervals can be completely filled by other tasks.
- No sorting or priority queue is required for this mathematical solution.

**Your implementation is correct and optimal.**
