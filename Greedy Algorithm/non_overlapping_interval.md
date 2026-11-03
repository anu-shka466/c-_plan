# Non-overlapping Intervals — LeetCode 435

## Pattern
- Greedy
- Sorting
- Interval Scheduling

## Approach

We need to remove the minimum number of intervals so that the remaining intervals do not overlap.

**Greedy idea:** Always keep the interval that finishes earliest.

Why? Because an interval that finishes earlier leaves more room for upcoming intervals.

### Steps

1. Sort intervals by their ending time in ascending order.
2. Initialize:
   - `ending = INT_MIN`
   - `remove = 0`
3. Traverse all intervals:
   - If `intervals[i][0] >= ending`, the current interval does not overlap with the previous selected interval.
     - Keep it.
     - Update `ending = intervals[i][1]`.
   - Otherwise, an overlap exists.
     - Remove the current interval.
     - Increment `remove`.
4. Return `remove`.

**Important:** We use `>=` because intervals that touch at a point are considered non-overlapping.

## Code (C++)

```cpp
bool compare(vector<int>& a, vector<int>& b) {
    return a[1] < b[1];
}

class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {

        sort(intervals.begin(), intervals.end(), compare);

        int ending = INT_MIN;
        int remove = 0;

        for (int i = 0; i < intervals.size(); i++) {

            if (ending <= intervals[i][0]) {
                // No overlap: keep this interval
                ending = intervals[i][1];
            }
            else {
                // Overlap: remove current interval
                remove++;
            }
        }

        return remove;
    }
};
```

## Dry Run

Input:

```cpp
intervals = {{1, 2}, {2, 3}, {3, 4}, {1, 3}};
```

After sorting by ending time:

```cpp
{{1, 2}, {2, 3}, {1, 3}, {3, 4}};
```

| Interval | Previous ending | Decision | New ending | Remove |
|---|---:|---|---:|---:|
| [1, 2] | INT_MIN | Keep | 2 | 0 |
| [2, 3] | 2 | Keep | 3 | 0 |
| [1, 3] | 3 | Remove | 3 | 1 |
| [3, 4] | 3 | Keep | 4 | 1 |

Final answer:

```cpp
1
```

## Understanding the Greedy Decision

Consider two overlapping intervals:

```text
A = [1, 5]
B = [2, 3]
```

Both overlap, so we can keep only one.

- If we keep A, the next interval must start at or after 5.
- If we keep B, the next interval can start at or after 3.

Therefore, keeping the interval that finishes earlier gives us more room for future intervals.

## Complexity

Let `n` be the number of intervals.

- Sorting: O(n log n)
- Traversal: O(n)

**Time:** O(n log n)

**Auxiliary Space:** O(1), excluding sorting implementation details.

## Key Takeaways

- This is the same greedy pattern as Activity Selection.
- Sort by ending time.
- Keep the interval that finishes earliest.
- If `start >= ending`, keep the interval.
- Otherwise, remove it.
- Your approach is **correct and optimal**.
