# Fractional Knapsack — Greedy Algorithm

## Pattern
- Greedy
- Sorting
- Value-to-Weight Ratio

## Approach

Your approach is **correct and optimal**. Instead of storing the ratio in the vector, you store `{value, weight}` and use a custom comparator to sort items by their ratio.

### 1. Understanding the Comparator

```cpp
bool compare(pair<int, int> a, pair<int, int> b) {
    return a.first * b.second > b.first * a.second;
}
```

We want to sort items in descending order of:

\[
\frac{value}{weight}
\]

For item A and item B, we want:

\[
\frac{a.first}{a.second} > \frac{b.first}{b.second}
\]

Cross-multiplying:

\[
a.first \times b.second > b.first \times a.second
\]

This avoids using floating-point division during sorting.

**Example:**

```cpp
A = {60, 10};   // Ratio = 6
B = {100, 20};  // Ratio = 5
```

Comparator:

```cpp
60 * 20 > 100 * 10
1200 > 1000  // true
```

Therefore, A comes before B.

### 2. Code (C++)

```cpp
bool compare(pair<int, int> a, pair<int, int> b) {
    return 1LL * a.first * b.second >
           1LL * b.first * a.second;
}

class Solution {
public:
    double fractionalKnapsack(vector<int>& val, vector<int>& wt, int w) {

        int n = val.size();
        vector<pair<int, int>> knapsack;

        for (int i = 0; i < n; i++) {
            knapsack.push_back({val[i], wt[i]});
        }

        sort(knapsack.begin(), knapsack.end(), compare);

        double profit = 0;
        int i = 0;

        while (w && i < n) {

            if (w >= knapsack[i].second) {
                profit += knapsack[i].first;
                w -= knapsack[i].second;
            }
            else {
                double temp = (double)knapsack[i].first /
                              knapsack[i].second;

                profit += temp * w;
                w = 0;
            }

            i++;
        }

        return profit;
    }
};
```

## Dry Run

Input:

```cpp
val = {60, 100, 120};
wt  = {10, 20, 30};
w = 50;
```

After sorting by value/weight ratio:

| Item | Value | Weight | Ratio |
|---|---:|---:|---:|
| 1 | 60 | 10 | 6 |
| 2 | 100 | 20 | 5 |
| 3 | 120 | 30 | 4 |

| Step | Action | Profit | Remaining Capacity |
|---|---|---:|---:|
| 1 | Take item 1 completely | 60 | 40 |
| 2 | Take item 2 completely | 160 | 20 |
| 3 | Take 20/30 of item 3 | 240 | 0 |

Final answer:

```cpp
240.000000
```

## Important: Why `1LL` in Comparator?

Your original comparator uses:

```cpp
a.first * b.second > b.first * a.second
```

Both operands are integers. If the multiplication exceeds the range of `int`, integer overflow can occur.

Using:

```cpp
1LL * a.first * b.second
```

converts the multiplication to `long long`, making it safer.

## Complexity

- Creating pairs: O(n)
- Sorting: O(n log n)
- Traversing: O(n)

**Time:** O(n log n)

**Auxiliary Space:** O(n)

## Key Takeaways

- Store `{value, weight}` in pairs.
- Sort by descending value/weight ratio.
- Cross multiplication avoids floating-point comparison.
- Use `1LL` to prevent integer overflow in comparator multiplication.
- Use `double` when calculating the fractional profit.
- Your greedy logic is correct.
