# Lemonade Change — LeetCode 860

**Pattern:** Greedy + Simulation

## 1. Is your code correct?

<badge color="success">Your main logic is correct</badge>

You have correctly handled both ways of giving `$15` change for a `$20` bill:

1. Give one `$10` + one `$5`.
2. Give three `$5` bills.

Your greedy choice of **giving `$10 + $5` first** is optimal because `$5` bills are more useful for giving change to `$10` customers.

### One improvement

This condition is unnecessary:

```cpp
if(n > 1 && bills[0] != 5 &&
   (bills[1] != 5 || bills[1] != 10))
    return false;
```

The expression `(bills[1] != 5 || bills[1] != 10)` is always true because a bill cannot be both 5 and 10.

You don't need this condition anyway. Your loop already handles the first customer's payment correctly.

## 2. Corrected code (your approach)

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five = 0;
        int ten = 0;

        for(int i = 0; i < bills.size(); i++) {

            if(bills[i] == 5) {
                five++;
            }
            else if(bills[i] == 10) {
                if(five >= 1) {
                    five--;
                    ten++;
                }
                else {
                    return false;
                }
            }
            else {  // bills[i] == 20

                if(five >= 1 && ten >= 1) {
                    ten--;
                    five--;
                }
                else if(five >= 3) {
                    five -= 3;
                }
                else {
                    return false;
                }
            }
        }

        return true;
    }
};
```

## 3. Understanding the greedy choice

A lemonade costs `$5`.

| Customer pays | Change required | What you do |
|---|---|---|
| $5 | $0 | Keep the $5 |
| $10 | $5 | Give one $5 |
| $20 | $15 | Prefer one $10 + one $5 |

**Why prefer `$10 + $5` over three `$5` bills?**

Because `$5` bills are more useful. You need them whenever a customer pays `$10`.

So preserve your `$5` bills whenever possible.

## 4. Dry run

Input:

```cpp
bills = [5, 5, 5, 10, 20]
```

Initially:

```cpp
five = 0;
ten = 0;
```

| Customer | Payment | Action | Five bills | Ten bills |
|---|---:|---|---:|---:|
| 1 | 5 | Keep $5 | 1 | 0 |
| 2 | 5 | Keep $5 | 2 | 0 |
| 3 | 5 | Keep $5 | 3 | 0 |
| 4 | 10 | Give $5 | 2 | 1 |
| 5 | 20 | Give $10 + $5 | 1 | 0 |

All customers receive correct change.

<badge color="success">Answer = true</badge>

## 5. Complexity analysis

- **Time Complexity:** O(N) — Visit every customer once.
- **Auxiliary Space:** O(1) — Only two counters are maintained.

## 6. Interview takeaway

The important greedy decision is:

```cpp
if(five >= 1 && ten >= 1) {
    ten--;
    five--;
}
else if(five >= 3) {
    five -= 3;
}
else {
    return false;
}
```

**Remember:** For a `$20` bill, prefer `$10 + $5` and use three `$5` bills only when necessary.
