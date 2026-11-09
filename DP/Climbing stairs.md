# Climbing Stairs — LeetCode 70

**Pattern:** Dynamic Programming + Recursion + Memoization

## 1. Is your code correct?

<badge color="success">Your logic is correct</badge>

You have correctly used **recursion with memoization**.

There is just one improvement: clear `dp` before starting a new call to `climbStairs()`. Otherwise, old values could be reused if the same `Solution` object is called with a different `n`.

## 2. Corrected code (your approach)

```cpp
class Solution {
public:
    unordered_map<int, int> dp;

    int climbStairs(int n) {
        dp.clear();
        return fun(0, n);
    }

    int fun(int i, int n) {
        if(i == n) return 1;
        if(i > n) return 0;

        if(dp.find(i) != dp.end()) {
            return dp[i];
        }

        int ans = fun(i+1, n) + fun(i+2, n);

        dp[i] = ans;
        return ans;
    }
};
```

## 3. Understanding the recursion

At every step, you have two choices:

- Take 1 step → `fun(i+1, n)`
- Take 2 steps → `fun(i+2, n)`

Therefore:

\[
f(i) = f(i+1) + f(i+2)
\]

**Base cases:**

```cpp
if(i == n) return 1;
if(i > n) return 0;
```

- `i == n`: We reached the top, so this is one valid way.
- `i > n`: We crossed the top, so this is not a valid way.

## 4. Dry run — n = 3

Possible ways:

```text
1 → 1 → 1
1 → 2
2 → 1
```

Let's calculate:

```text
fun(0, 3)
    |
    |--- fun(1, 3)
    |       |
    |       |--- fun(2, 3)
    |       |       |
    |       |       |--- fun(3, 3) = 1
    |       |       |--- fun(4, 3) = 0
    |       |
    |       |--- fun(3, 3) = 1
    |
    |--- fun(2, 3) = 2  // Retrieved from dp
```

So:

\[
f(2)=1,\quad f(1)=2,\quad f(0)=3
\]

<badge color="success">Answer = 3</badge>

## 5. Complexity analysis

| Complexity | Value | Reason |
|---|---|---|
| Time | O(n) | Each state is calculated once |
| Space | O(n) | Memoization map + recursion stack |

## 6. Interview takeaway

This is the Fibonacci pattern:

\[
dp[n] = dp[n-1] + dp[n-2]
\]

The difference is that here we are counting the number of ways to reach the top rather than directly calculating a Fibonacci number.

**Remember:** Whenever you see a problem asking for the number of distinct ways, consider recursion with pick/choice decisions and then optimize using DP.
