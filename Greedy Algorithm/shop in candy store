# Shop in Candy Store — Greedy Algorithm

## Pattern
- Greedy
- Sorting
- Two Pointers

## Approach

We need to calculate two things:
1. **Minimum cost:** Buy the cheapest candies and get the most expensive candies for free.
2. **Maximum cost:** Buy the most expensive candies and get the cheapest candies for free.

Since buying one candy gives us up to `k` other candies for free, we should always take exactly `k` free candies whenever possible.

### 1. Minimum Cost

1. Sort prices in ascending order.
2. Buy the cheapest candy: `prices[i]`.
3. Take the next `k` most expensive candies for free by reducing `j` by `k`.
4. Repeat until all candies are covered.

### 2. Maximum Cost

1. Start from the most expensive candy.
2. Buy the most expensive candy: `prices[i]`.
3. Take the next `k` cheapest candies for free by increasing `j` by `k`.
4. Repeat until all candies are covered.

## Code (C++)

```cpp
class Solution {
public:
    vector<int> minMaxCandy(vector<int>& prices, int k) {
        sort(prices.begin(), prices.end());

        int min_coins = 0;
        int max_coins = 0;

        int i = 0, j = prices.size() - 1;

        // Minimum cost
        while (i <= j) {
            min_coins += prices[i];
            i++;
            j -= k;
        }

        // Maximum cost
        i = prices.size() - 1;
        j = 0;

        while (i >= j) {
            max_coins += prices[i];
            i--;
            j += k;
        }

        return {min_coins, max_coins};
    }
};
```

## Dry Run

Input:

```cpp
prices = {3, 2, 1, 4};
k = 2;
```

After sorting:

```cpp
prices = {1, 2, 3, 4};
```

### Minimum Cost

| Step | Bought candy | Free candies | Cost |
|---|---:|---|---:|
| 1 | 1 | 4, 3 | 1 |
| 2 | 2 | None | 2 |

Total minimum cost:

```cpp
1 + 2 = 3
```

### Maximum Cost

| Step | Bought candy | Free candies | Cost |
|---|---:|---|---:|
| 1 | 4 | 1, 2 | 4 |
| 2 | 3 | None | 3 |

Total maximum cost:

```cpp
4 + 3 = 7
```

Final answer:

```cpp
{3, 7}
```

## Understanding the Two Pointers

For minimum cost:

```cpp
int i = 0, j = prices.size() - 1;
```

- `i` points to the cheapest candy we will buy.
- `j` points to the most expensive candy we can take for free.
- `i++` moves to the next cheapest candy.
- `j -= k` skips up to `k` expensive free candies.

For maximum cost:

```cpp
i = prices.size() - 1;
j = 0;
```

- `i` points to the most expensive candy we will buy.
- `j` points to the cheapest candy we can take for free.
- `i--` moves to the next expensive candy.
- `j += k` skips up to `k` cheap free candies.

## Complexity

Let `n` be the number of candies.

- Sorting: O(n log n)
- Minimum cost calculation: O(n)
- Maximum cost calculation: O(n)

**Total Time:** O(n log n)

**Auxiliary Space:** O(1), excluding sorting implementation details.

## Key Takeaways

- For minimum cost, buy the cheapest and get the most expensive for free.
- For maximum cost, buy the most expensive and get the cheapest for free.
- Sorting makes both greedy strategies possible.
- Each purchase covers at most `k + 1` candies.
- Your approach is **correct and optimal**.
