# 0/1 Knapsack — Bottom-Up DP (Tabulation)

**Pattern:** Dynamic Programming + Tabulation

## 1. Is your code correct?

<badge color="success">Your code is 100% correct</badge>

You have successfully converted your previous **Top-Down DP (Memoization)** solution into **Bottom-Up DP (Tabulation)**.

Your implementation correctly handles:
- Including an item.
- Excluding an item.
- Items that exceed the remaining capacity.
- The base case when all items have been considered.

## 2. Understanding your approach

### Step 1: Initialize the DP array

```cpp
vector<vector<int>> dp(n+1, vector<int>(w+1, -1));
```

Why `n+1` rows?

Because we need an additional row representing the situation where all items have been considered.

Why `w+1` columns?

Because capacity ranges from `0` to `w`, inclusive.

### Step 2: Base case

```cpp
for(int j = 0; j <= w; j++) {
    dp[n][j] = 0;
}
```

When all items are considered, the maximum value is zero, regardless of the remaining capacity.

### Step 3: Fill the DP table

```cpp
for(int i = n-1; i >= 0; i--) {
    for(int j = 0; j <= w; j++) {
```

We iterate backwards over the items because our current state depends on the next row: `dp[i+1]`.

Here:
- `i` = current item.
- `j` = available capacity.
- `dp[i][j]` = maximum value obtainable using items from index `i` onwards.

### Step 4: Item doesn't fit

```cpp
if(wt[i] > j) {
    dp[i][j] = dp[i+1][j];
}
```

We cannot include the current item, so we skip it.

### Step 5: Item fits

```cpp
dp[i][j] = max(
    val[i] + dp[i+1][j-wt[i]],
    dp[i+1][j]
);
```

Two choices:

**Include the item:**
- Add `val[i]`.
- Reduce capacity by `wt[i]`.
- Move to the next item.

**Exclude the item:**
- Don't add its value.
- Keep the same capacity.
- Move to the next item.

Choose the maximum of both.

## 3. Dry run

Consider:

```cpp
W = 4;
val = {1, 2, 3};
wt  = {4, 5, 1};
```

The DP table will be:

| Item index | Capacity 0 | Capacity 1 | Capacity 2 | Capacity 3 | Capacity 4 |
|---|---:|---:|---:|---:|---:|
| 3 (base row) | 0 | 0 | 0 | 0 | 0 |
| 2 | 0 | 3 | 3 | 3 | 3 |
| 1 | 0 | 3 | 3 | 3 | 3 |
| 0 | 0 | 3 | 3 | 3 | 3 |

Finally:

```cpp
return dp[0][4];
```

Therefore:

<badge color="success">Answer = 3</badge>

We select the third item, which has weight `1` and value `3`.

## 4. Complexity analysis

| Complexity | Value |
|---|---|
| Time | O(n × W) |
| Auxiliary Space | O(n × W) |

Every DP state is calculated exactly once.

## 5. Memoization vs Tabulation

| Top-Down (Your previous solution) | Bottom-Up (Current solution) |
|---|---|
| Uses recursion | Uses loops |
| Starts from index 0 | Starts filling from index n-1 |
| Calculates states when needed | Calculates states systematically |
| Uses recursion stack | No recursion stack |
| Uses 2D DP | Uses 2D DP |

## 6. Important interview takeaway

Your recurrence is:

\[
dp[i][j] =
\max(val[i]+dp[i+1][j-wt[i]],\ dp[i+1][j])
\]

When the item doesn't fit:

\[
dp[i][j] = dp[i+1][j]
\]

**One important observation:** Because you are using `dp[i+1]` for both choices, the loop over `i` must run backwards.

Your transition and loop directions are both correct!
