# Target Sum — GFG

**Pattern:** Dynamic Programming (0/1 Knapsack — Count Subsets with Given Sum)

## Is your code correct?

Your DP recurrence and subset-sum conversion are correct. However, **there is one condition you need to fix**.

You wrote:

```cpp
if(total < target) return 0;
```

This handles targets greater than `total`, but does not handle targets smaller than `-total`.

For example:
- `arr = {1, 2, 3}`, `target = -7`
- `total = 6`
- No expression can produce `-7`.

So use:

```cpp
if(target > total || target < -total) return 0;
```

## Corrected code

```cpp
class Solution {
public:
    int totalWays(vector<int>& arr, int target) {
        int n = arr.size();

        int total = 0;
        for(int i = 0; i < n; i++) {
            total += arr[i];
        }

        // Target must be between -total and +total
        if(target > total || target < -total) return 0;

        // Subset sum transformation
        if((total + target) % 2 != 0) return 0;

        int sum = (total + target) / 2;

        vector<vector<int>> dp(
            n + 1, vector<int>(sum + 1, 0)
        );

        // Base case: empty subset makes sum 0 in one way
        dp[n][0] = 1;

        for(int i = n - 1; i >= 0; i--) {
            for(int j = 0; j <= sum; j++) {

                // Exclude current element
                dp[i][j] = dp[i + 1][j];

                // Include current element
                if(arr[i] <= j) {
                    dp[i][j] += dp[i + 1][j - arr[i]];
                }
            }
        }

        return dp[0][sum];
    }
};
```

## Why are we converting it into Subset Sum?

Suppose:
- `P` = sum of elements having `+` sign.
- `N` = sum of elements having `-` sign.
- `total` = sum of all elements.

We know:

```text
P - N = target       ...(1)
P + N = total        ...(2)
```

Adding both equations:

```text
2P = total + target

P = (total + target) / 2
```

Therefore, our problem becomes:

**Count the number of subsets whose sum is `(total + target) / 2`.**

That's exactly the Count Subsets with Given Sum problem.

## Dry run

```cpp
arr = {1, 1, 1, 1, 1}
target = 3
```

Total:

```text
total = 5

sum = (5 + 3) / 2
    = 4
```

Now count subsets with sum `4`.

There are 5 possible subsets because we can select any four of the five elements.

For example:

```text
+1 +1 +1 +1 -1 = 3
+1 +1 +1 -1 +1 = 3
+1 +1 -1 +1 +1 = 3
+1 -1 +1 +1 +1 = 3
-1 +1 +1 +1 +1 = 3
```

Therefore:

```text
Answer = 5
```

## Understand the DP transition

```cpp
dp[i][j] = dp[i + 1][j];
```

This counts ways by excluding the current element.

```cpp
if(arr[i] <= j) {
    dp[i][j] += dp[i + 1][j - arr[i]];
}
```

This adds ways by including the current element.

We use `+` instead of `||` because we need to **count all possible subsets**, not just check whether a subset exists.

**Important:** If an element is `0`, including or excluding it gives the same sum, but represents two different choices. The recurrence correctly counts both.

## Complexity

- **Time:** O(n × sum)
- **Space:** O(n × sum)

Here, `sum = (total + target) / 2`.

## Connection with your previous problems

| Problem | DP operation |
|---|---|
| Subset Sum | Check whether a subset exists |
| Count Subsets | Count subsets with a given sum |
| Target Sum | Convert into Count Subsets |

The main difference is:

- Subset Sum → `||`
- Count Subsets → `+`

You have correctly identified the connection between Target Sum and Count Subsets!
