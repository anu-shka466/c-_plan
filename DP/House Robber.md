# House Robber — LeetCode 198

**Pattern:** Dynamic Programming + Recursion + Memoization (Top-Down DP)

## 1. Is your code correct?

<badge color="success">Your code is correct</badge>

Your approach is good! You have used a 2D DP array to remember two possible states at every house.

One small improvement: You can initialize your DP array directly instead of using a separate loop.

## 2. Understanding your approach

Your function:

```cpp
func(nums, n, i, free, dp)
```

Here:

- `i` = current house index.
- `free = 1` = You are allowed to rob the current house.
- `free = 0` = You must skip the current house because you robbed the previous one.

At every house, you have two choices:

**Case 1: Previous house was robbed (`free == 0`)**

You cannot rob the current house.

```cpp
return dp[i][free] = func(nums, n, i+1, 1, dp);
```

**Case 2: You are allowed to rob (`free == 1`)**

You have two choices:

- Rob current house: Add `nums[i]` and move to the next house with `free = 0`.
- Skip current house: Move to the next house with `free = 1`.

```cpp
int c1 = nums[i] + func(nums, n, i+1, 0, dp);
int c2 = func(nums, n, i+1, 1, dp);

return dp[i][free] = max(c1, c2);
```

## 3. Corrected code (your approach)

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();

        vector<vector<int>> dp(n, vector<int>(2, -1));

        return func(nums, n, 0, 1, dp);
    }

    int func(vector<int>& nums, int n, int i, int free,
             vector<vector<int>>& dp) {

        if(i == n) return 0;

        if(dp[i][free] != -1)
            return dp[i][free];

        if(free == 0) {
            return dp[i][free] =
                func(nums, n, i+1, 1, dp);
        }

        int c1 = nums[i] + func(nums, n, i+1, 0, dp);

        int c2 = func(nums, n, i+1, 1, dp);

        return dp[i][free] = max(c1, c2);
    }
};
```

## 4. Dry run

Consider:

```cpp
nums = [1, 2, 3, 1]
```

Initially:

```cpp
func(nums, 4, 0, 1, dp);
```

We can either rob or skip each house.

| House | Money | Decision |
|---|---:|---|
| 1 | 1 | Rob |
| 2 | 2 | Skip |
| 3 | 3 | Rob |
| 4 | 1 | Skip |

Total:

\[
1 + 3 = 4
\]

<badge color="success">Answer = 4</badge>

## 5. Complexity analysis

| Complexity | Value | Reason |
|---|---|---|
| Time | O(n) | Two states per house, each calculated once |
| Auxiliary Space | O(n) | DP array + recursion stack |

## 6. Important interview takeaway

Your recurrence is:

\[
dp[i][1] = \max(nums[i]+dp[i+1][0],\ dp[i+1][1])
\]

\[
dp[i][0] = dp[i+1][1]
\]

**Remember:** The main idea is to track whether the previous house was robbed. This prevents robbing two adjacent houses.

Your solution is a valid Top-Down DP solution using a 2D state.
