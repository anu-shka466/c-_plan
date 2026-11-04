# Minimum Coins — Greedy Algorithm

## Pattern
- Greedy
- Coin Change

## Approach

We need to find the minimum number of coins required to make the value `n`.

Available denominations:

```cpp
{1, 2, 5, 10}
```

**Greedy idea:** Always choose the largest denomination possible first.

### Steps

1. Store denominations in descending order: `{10, 5, 2, 1}`.
2. Traverse the denominations.
3. For each denomination:
   - Calculate how many coins can be used: `n / coins[i]`.
   - Add that count to `mincoin`.
   - Update the remaining amount: `n = n % coins[i]`.
4. Continue until `n` becomes zero.
5. Return `mincoin`.

## Code (C++)

```cpp
class Solution {
public:
    int findMin(int n) {
        int coins[] = {10, 5, 2, 1};
        int mincoin = 0;
        int i = 0;

        while (n) {
            mincoin += n / coins[i];
            n = n % coins[i];
            i++;
        }

        return mincoin;
    }
};
```

## Dry Run

Input:

```cpp
n = 39;
```

| Denomination | Calculation | Coins Used | Remaining Amount |
|---|---|---:|---:|
| 10 | 39 / 10 | 3 | 9 |
| 5 | 9 / 5 | 1 | 4 |
| 2 | 4 / 2 | 2 | 0 |
| 1 | 0 / 1 | 0 | 0 |

Total coins:

```cpp
3 + 1 + 2 = 6
```

Final answer:

```cpp
6
```

## Understanding the Operators

For denomination `10` and amount `39`:

```cpp
39 / 10 = 3
```

This tells us we can use three 10-value coins.

```cpp
39 % 10 = 9
```

This gives the remaining amount after using those coins.

## Complexity

There are always four denominations.

- **Time:** O(1)
- **Space:** O(1)

## Key Takeaways

- Greedy means choosing the largest possible denomination at every step.
- Integer division (`/`) tells us how many coins we can use.
- Modulus (`%`) gives the remaining amount.
- Your approach is **correct for the given denominations `{1, 2, 5, 10}`**.

**Important interview point:** Greedy does not work for every possible coin system. For example, with denominations `{1, 3, 4}` and amount `6`, greedy chooses `4 + 1 + 1` (3 coins), while the optimal answer is `3 + 3` (2 coins). 
