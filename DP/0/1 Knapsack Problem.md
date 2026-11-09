# 0/1 Knapsack Problem — GFG

**Pattern:** Dynamic Programming + Recursion + Memoization (Top-Down DP)

## 1. Is your code correct?

<badge color="success">Your code is correct</badge>

You have correctly implemented the **0/1 Knapsack** problem using a 2D DP array.

Your approach considers two choices for every item:
- Include the item.
- Exclude the item.

Since each item can be selected only once, you move to `i+1` in both recursive calls. This is correct.

## 2. Understanding your approach

Your function:

```cpp
func(val, n, wt, i, w, dp)
```

Here:
- `i` = current item index.
- `w` = remaining knapsack capacity.
- `dp[i][w]` = maximum value possible using items from index `i` onwards with capacity `w`.

**Base case:**

```cpp
if(i == n) return 0;
```

If all items have been considered, no more value can be added.

**Case 1: Item weight exceeds capacity**

```cpp
if(wt[i] > w) {
    return dp[i][w] = func(val, n, wt, i+1, w, dp);
}
```

We cannot include this item, so we skip it.

**Case 2: Item fits in the knapsack**

We have two choices:

Include:
```cpp
int c1 = val[i] + func(val, n, wt, i+1, w-wt[i], dp);
```

- Add its value.
- Reduce the remaining capacity.
- Move to the next item.

Exclude:
```cpp
int c2 = func(val, n, wt, i+1, w, dp);
```

- Don't add its value.
- Capacity remains unchanged.
- Move to the next item.

Finally:

```cpp
return dp[i][w] = max(c1, c2);
```

Choose whichever gives the maximum value.

## 3. Corrected code (your approach)

Only a small initialization improvement is needed:

```cpp
class Solution {
public:
    int knapsack(int w, vector<int>& val, vector<int>& wt) {
        int n = val.size();

        vector<vector<int>> dp(n, vector<int>(w+1, -1));

        return func(val, n, wt, 0, w, dp);
    }

    int func(vector<int>& val, int n, vector<int>& wt,
             int i, int w, vector<vector<int>>& dp) {

        if(i == n) return 0;

        if(dp[i][w] != -1)
            return dp[i][w];

        if(wt[i] > w) {
            return dp[i][w] =
                func(val, n, wt, i+1, w, dp);
        }

        int c1 = val[i] +
                 func(val, n, wt, i+1, w-wt[i], dp);

        int c2 = func(val, n, wt, i+1, w, dp);

        return dp[i][w] = max(c1, c2);
    }
};
```

## 4. Dry run

Given:

```cpp
W = 4
val = [1, 2, 3]
wt  = [4, 5, 1]
```

| Item | Weight | Value | Decision |
|---|---:|---:|---|
| 0 | 4 | 1 | Include or exclude |
| 1 | 5 | 2 | Cannot include |
| 2 | 1 | 3 | Include if capacity permits |

Let's compare the choices:

**Choice 1: Include item 0**

- Remaining capacity = 4 - 4 = 0
- Total value = 1

**Choice 2: Exclude item 0**

- Remaining capacity = 4
- Item 1 cannot fit because its weight is 5.
- Include item 2: value = 3.

Therefore:

\[
\max(1,3)=3
\]

<badge color="success">Answer = 3</badge>

## 5. Complexity analysis

| Complexity | Value | Reason |
|---|---|---|
| Time | O(n × W) | At most n × (W+1) DP states |
| Auxiliary Space | O(n × W) | 2D DP array |
| Recursion Stack | O(n) | Maximum recursion depth |

## 6. Important interview takeaway

The recurrence is:

\[
dp[i][w] =
\max(val[i]+dp[i+1][w-wt[i]],\ dp[i+1][w])
\]

This applies when the current item's weight fits.

Remember the difference:

| 0/1 Knapsack | Unbounded Knapsack |
|---|---|
| Each item used once | Item can be reused |
| Include → `i+1` | Include → `i` |
| Exclude → `i+1` | Exclude → `i+1` |

**Your solution is a correct Top-Down DP implementation of 0/1 Knapsack.**
