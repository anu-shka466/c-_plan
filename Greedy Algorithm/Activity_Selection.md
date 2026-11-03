# Activity Selection — Greedy Algorithm

## Pattern
- Greedy
- Sorting
- Interval Scheduling

## Approach

Our goal is to select the maximum number of non-overlapping activities.

**Greedy idea:** Always select the activity that finishes earliest.

Why?

Because an activity that finishes earlier leaves more time available for the remaining activities.

### Steps

1. Create a vector of pairs `{start[i], finish[i]}`.
2. Sort the activities in ascending order of their finish time.
3. Initialize:
   - `count = 0`
   - `ending = -1`
4. Traverse the sorted activities:
   - If `start[i] > ending`, select the activity.
   - Increment `count`.
   - Update `ending = finish[i]`.
5. Return `count`.

**Important:** We use `>` instead of `>=` because the next activity must start strictly after the previous activity finishes.

## Code (C++)

```cpp
bool compare(pair<int,int> a, pair<int,int> b) {
    return a.second < b.second;
}

class Solution {
public:
    int activitySelection(vector<int>& start, vector<int>& finish) {
        vector<pair<int,int>> meetings;

        for (int i = 0; i < start.size(); i++) {
            meetings.push_back({start[i], finish[i]});
        }

        sort(meetings.begin(), meetings.end(), compare);

        int count = 0;
        int ending = -1;

        for (int i = 0; i < meetings.size(); i++) {
            if (meetings[i].first > ending) {
                count++;
                ending = meetings[i].second;
            }
        }

        return count;
    }
};
```

## Dry Run

Input:

```cpp
start  =  {1, 3, 0, 5, 8, 5};
finish =  {2, 4, 6, 7, 9, 9};
```

After sorting by finish time:

| Activity | Start | Finish | Decision |
|---|---:|---:|---|
| 0 | 1 | 2 | Select |
| 1 | 3 | 4 | Select |
| 2 | 0 | 6 | Reject |
| 3 | 5 | 7 | Select |
| 4 | 8 | 9 | Select |
| 5 | 5 | 9 | Reject |

| Step | Start | Finish | Previous ending | Decision |
|---|---:|---:|---:|---|
| 1 | 1 | 2 | -1 | Select |
| 2 | 3 | 4 | 2 | Select |
| 3 | 0 | 6 | 4 | Reject |
| 4 | 5 | 7 | 4 | Select |
| 5 | 8 | 9 | 7 | Select |
| 6 | 5 | 9 | 9 | Reject |

Final answer:

```cpp
4
```

## Why Sort by Finish Time?

Suppose we have two activities:

```text
Activity A: (1, 5)
Activity B: (2, 3)
```

If we select A first, we finish at time 5.

If we select B first, we finish at time 3, leaving more time for other activities.

Therefore, choosing the activity with the earliest finish time is the optimal greedy strategy.

## Understanding the Comparator

```cpp
bool compare(pair<int,int> a, pair<int,int> b) {
    return a.second < b.second;
}
```

- `a.second` represents the finish time of activity A.
- `b.second` represents the finish time of activity B.
- Returning `true` means A should come before B.

This sorts activities by finish time in ascending order.

## Complexity

Let `n` be the number of activities.

- Creating pairs: O(n)
- Sorting: O(n log n)
- Traversal: O(n)

**Total Time:** O(n log n)

**Space:** O(n) for storing the activity pairs.

## Key Takeaways

- This is a classic Greedy problem.
- Always select the activity with the earliest finish time.
- Sort by finish time, not start time.
- Select the next activity only if `start > ending`.
- Your approach is **correct and optimal**.
