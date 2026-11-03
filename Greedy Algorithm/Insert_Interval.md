# Insert Interval — LeetCode 57

## Pattern
Greedy + Interval Merging

## Approach

The intervals are already sorted by starting time and are non-overlapping. So, we don't need to sort them again.

We process the intervals in three phases:

1. **Add intervals before `newInterval`:**
   - If `intervals[i][1] < newstart`, the current interval ends before the new interval starts.
   - Add it directly to `ans`.

2. **Merge overlapping intervals:**
   - If `intervals[i][0] <= newend`, the current interval overlaps with the new interval.
   - Update:
     - `newstart = min(intervals[i][0], newstart)`
     - `newend = max(intervals[i][1], newend)`
   - Continue because the expanded interval might overlap with more intervals.

3. **Add the merged interval and remaining intervals:**
   - Insert `{newstart, newend}` into `ans`.
   - Add all remaining intervals directly.

## Code (C++)

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals,
                               vector<int>& newInterval) {

        int n = intervals.size();

        if (n == 0) return {newInterval};

        int newstart = newInterval[0];
        int newend = newInterval[1];

        int i = 0;
        vector<vector<int>> ans;

        // 1. Add intervals before newInterval
        while (i < n && intervals[i][1] < newstart) {
            ans.push_back(intervals[i]);
            i++;
        }

        // 2. Merge overlapping intervals
        while (i < n && intervals[i][0] <= newend) {
            newstart = min(intervals[i][0], newstart);
            newend = max(intervals[i][1], newend);
            i++;
        }

        // Add merged interval
        ans.push_back({newstart, newend});

        // 3. Add remaining intervals
        while (i < n) {
            ans.push_back(intervals[i]);
            i++;
        }

        return ans;
    }
};
```

## Dry Run

**Input:**

```cpp
intervals = {{1,2}, {3,5}, {6,7}, {8,10}, {12,16}};
newInterval = {4,8};
```

Initially:

```cpp
newstart = 4;
newend = 8;
```

| Current interval | Condition / Action | Updated interval |
|---|---|---|
| `[1,2]` | `2 < 4`, add directly | `[4,8]` |
| `[3,5]` | `3 <= 8`, merge | `[3,8]` |
| `[6,7]` | `6 <= 8`, merge | `[3,8]` |
| `[8,10]` | `8 <= 8`, merge | `[3,10]` |
| `[12,16]` | `12 <= 10` is false | Stop merging |

Finally, add `[3,10]` and the remaining `[12,16]`.

**Output:**

```cpp
{{1,2}, {3,10}, {12,16}}
```

## Why do we use different conditions?

### Condition 1

```cpp
intervals[i][1] < newstart
```

The current interval ends before the new interval starts.

We use strict `<` because sharing an endpoint means the intervals overlap.

### Condition 2

```cpp
intervals[i][0] <= newend
```

The current interval starts before or exactly at the merged interval's ending point.

Therefore, it overlaps and must be merged.

## Complexity Analysis

- **Time Complexity:** `O(n)` — Every interval is visited at most once.
- **Space Complexity:** `O(n)` — The answer array stores the resulting intervals.

## Key Takeaways

- The input is already sorted, so sorting is unnecessary.
- Use three phases: add before, merge, add after.
- Update both start and end while merging.
- Use `<=` when checking overlap because touching endpoints count as overlapping.
- The `n == 0` condition is optional; the remaining code handles an empty array too.

**Your implementation is correct and optimal: `O(n)` time.**
