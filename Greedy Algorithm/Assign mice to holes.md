# Assign Mice to Holes — Greedy Algorithm

## Pattern
- Greedy
- Sorting
- Minimize the Maximum Distance

## Approach

Our goal is to minimize the time taken by the **last mouse** to reach its assigned hole.

**Greedy idea:** Sort both arrays and assign the mouse at index `i` to the hole at index `i`.

### Steps

1. Sort `mices[]` in ascending order.
2. Sort `holes[]` in ascending order.
3. Assign each mouse to the hole at the same index.
4. Calculate the distance for each pair:
   `abs(mices[i] - holes[i])`
5. Return the maximum distance among all pairs.

Why maximum?

All mice move simultaneously, so the total time is determined by the mouse that takes the longest time to reach its hole.

## Code (C++)

```cpp
class Solution {
public:
    int assignHole(vector<int>& mices, vector<int>& holes) {
        sort(mices.begin(), mices.end());
        sort(holes.begin(), holes.end());

        int ans = INT_MIN;

        for (int i = 0; i < mices.size(); i++) {
            ans = max(ans, abs(mices[i] - holes[i]));
        }

        return ans;
    }
};
```

## Dry Run

Input:

```cpp
mices = {4, -4, 2};
holes = {4, 0, 5};
```

After sorting:

```cpp
mices = {-4, 2, 4};
holes = {0, 4, 5};
```

Assign mice and holes by index:

| Mouse | Hole | Distance |
|---:|---:|---:|
| -4 | 0 | 4 |
| 2 | 4 | 2 |
| 4 | 5 | 1 |

Maximum distance:

```cpp
max(4, 2, 1) = 4
```

Final answer:

```cpp
4
```

## Why Does Sorting Work?

Imagine two mice and two holes:

```text
Mouse positions:  1,  10
Hole positions:   2,   9
```

Matching in sorted order:

```text
1  -> 2   (distance 1)
10 -> 9   (distance 1)
```

If we cross the assignments:

```text
1  -> 9   (distance 8)
10 -> 2   (distance 8)
```

The crossed assignment creates a larger maximum distance.

In general, matching mice and holes in sorted order avoids unnecessary crossing assignments and minimizes the maximum distance.

## Complexity

Let `n` be the number of mice.

- Sorting mice: O(n log n)
- Sorting holes: O(n log n)
- Finding maximum distance: O(n)

**Total Time:** O(n log n)

**Auxiliary Space:** O(1) for the matching logic, excluding sorting implementation details.

## Key Takeaways

- Sort both arrays.
- Match elements at the same index.
- Use absolute difference because mice can move in either direction.
- Take the maximum distance, not the sum.
- Your approach is **correct and optimal**.
