# Subset Sum Problem — GFG

**Pattern:** Dynamic Programming (0/1 Knapsack — Subset Sum)

## Is your code correct?

<badge color="success">Correct</badge>

Your tabulation logic is absolutely correct. You are considering two choices for every element:

1. **Include** the current element in the subset.
2. **Exclude** the current element from the subset.

One small improvement: instead of initializing the DP table with `-1`, initialize it with `false`. Since `dp` is a `bool` table, `-1` converts to `true`.

## Cleaned-up code

```cpp
class Solution {
public:
    bool isSubsetSum(vector<int>& arr, int sum) {
        int n = arr.size();

        vector<vector<bool>> dp(
            n + 1, vector<bool>(sum + 1, false)
        );

        // Base case: empty subset gives sum 0
        dp[n][0] = true;

        for (int i = n - 1; i >= 0; i--) {
            for (int j = 0; j <= sum; j++) {

                // Exclude current element
                if (arr[i] > j) {
                    dp[i][j] = dp[i + 1][j];
                }
                else {
                    // Include OR exclude current element
                    dp[i][j] = dp[i + 1][j - arr[i]]
                               || dp[i + 1][j];
                }
            }
        }

        return dp[0][sum];
    }
};
```

## Understand the DP state

`dp[i][j]` means:

**Can we make sum `j` using elements from index `i` to `n-1`?**

### Base case

```cpp
dp[n][0] = true;
```

Why?

- With no elements, we can make sum `0` by selecting an empty subset.
- With no elements, we cannot make any positive sum.

All other cells in the last row are therefore `false`.

### Recurrence

When `arr[i] <= j`:

```cpp
dp[i][j] = dp[i+1][j-arr[i]] || dp[i+1][j];
```

- `dp[i+1][j-arr[i]]`: Include the current element.
- `dp[i+1][j]`: Exclude the current element.

We use `||` because **either choice being true is enough**.

When `arr[i] > j`, we cannot include the element:

```cpp
dp[i][j] = dp[i+1][j];
```

## Dry run

Input:

```cpp
arr = {3, 34, 4, 12, 5, 2}
sum = 9
```

One valid subset is:

```text
3 + 4 + 2 = 9
```

Therefore:

```cpp
dp[0][9] = true;
```

And the function returns `true`.

## Complexity

- **Time:** O(n × sum) — every DP cell is calculated once.
- **Space:** O(n × sum) — the DP table contains `(n+1) × (sum+1)` cells.

**Important:** This is the same 0/1 Knapsack pattern you used earlier. The difference is that instead of maximizing a value, we are checking whether a particular sum is possible.
