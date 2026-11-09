# Fibonacci Number — LeetCode 509

**Pattern:** Dynamic Programming + Recursion + Memoization

## 1. Is your code correct?

<badge color="success">Your code is correct</badge>

You have correctly implemented **Top-Down DP (Memoization)**.

No changes are required for the given LeetCode constraints.

## 2. Understanding your approach

The Fibonacci formula is:

\[
F(n) = F(n-1) + F(n-2)
\]

Your recursive function follows exactly this formula.

```cpp
class Solution {
public:
    unordered_map<int, int> dp;

    int fib(int n) {
        if(n == 0) return 0;
        if(n == 1) return 1;

        if(dp.find(n) != dp.end())
            return dp[n];

        int ans = fib(n-1) + fib(n-2);

        dp[n] = ans;

        return ans;
    }
};
```

**How memoization works:**

- First, check the base cases.
- Check whether `dp` already contains the answer for `n`.
- If yes, return the stored answer.
- Otherwise, calculate `fib(n-1) + fib(n-2)`.
- Store the result in `dp[n]` before returning it.

## 3. Dry run — n = 4

The recursion calculates:

```text
fib(4)
   |
   |--- fib(3)
   |      |
   |      |--- fib(2)
   |      |      |
   |      |      |--- fib(1) = 1
   |      |      |--- fib(0) = 0
   |      |
   |      |--- fib(1) = 1
   |
   |--- fib(2)  // Already stored in dp
```

Calculation:

| n | Fibonacci value |
|---|---:|
| 0 | 0 |
| 1 | 1 |
| 2 | 1 |
| 3 | 2 |
| 4 | 3 |

<badge color="success">Answer = 3</badge>

## 4. Complexity analysis

| Complexity | Value |
|---|---|
| Time | O(n) |
| Auxiliary Space | O(n) |

**Why O(n) time?**

Without memoization, the same Fibonacci values are calculated repeatedly.

With memoization, each value is calculated only once and reused whenever needed.

## 5. Important interview concept

There are two common DP approaches:

- **Top-Down DP:** Recursion + Memoization (your approach).
- **Bottom-Up DP:** Iteration + Tabulation.

Your solution is Top-Down DP.

**Key difference from Climbing Stairs:**

Both problems use the same recurrence:

\[
f(n)=f(n-1)+f(n-2)
\]

But their base cases are different:

| Problem | Base cases |
|---|---|
| Fibonacci | F(0) = 0, F(1) = 1 |
| Climbing Stairs | f(0) = 1, f(1) = 1 |
