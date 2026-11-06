# Sum of Digits — GFG

## Pattern
Recursion + Mathematics

## Approach

Your solution uses recursion to calculate the sum of all digits of a number.

We use two operations:

- `n % 10` → Extracts the last digit.
- `n / 10` → Removes the last digit (integer division).

### Step 1: Base Case

```cpp
if(n == 0) return 0;
```

When the number becomes `0`, there are no more digits to process.

Return `0`.

### Step 2: Extract the Last Digit

```cpp
int d = n % 10;
```

For example:

```cpp
n = 687;
d = 687 % 10 = 7;
```

### Step 3: Recursive Call

```cpp
return d + sumOfDigits(n / 10);
```

Remove the last digit and recursively calculate the sum of the remaining digits.

## Code (C++)

```cpp
class Solution {
public:
    int sumOfDigits(int n) {

        // Base case
        if(n == 0) return 0;

        // Extract last digit
        int d = n % 10;

        // Recursive call
        return d + sumOfDigits(n / 10);
    }
};
```

Your implementation is correct.

## Dry Run

**Input:**

```cpp
n = 687;
```

### Recursive Calls

```cpp
sumOfDigits(687)
= 7 + sumOfDigits(68)

= 7 + 8 + sumOfDigits(6)

= 7 + 8 + 6 + sumOfDigits(0)

= 7 + 8 + 6 + 0

= 21
```

### Recursion Tree

```text
sumOfDigits(687)
       |
    7 + sumOfDigits(68)
              |
           8 + sumOfDigits(6)
                     |
                  6 + sumOfDigits(0)
                              |
                              0
```

The recursive calls return:

```text
sumOfDigits(0)   = 0
sumOfDigits(6)   = 6
sumOfDigits(68)  = 14
sumOfDigits(687) = 21
```

**Output:**

```cpp
21
```

## Complexity Analysis

Let `d` be the number of digits in `n`.

- **Time Complexity:** `O(d)` — Each recursive call processes one digit.
- **Auxiliary Space Complexity:** `O(d)` — Recursive call stack.

Since the number of digits is approximately `log10(n)`, time complexity can also be written as `O(log n)`.

## Key Takeaways

- `% 10` extracts the last digit.
- `/ 10` removes the last digit.
- Add the extracted digit to the recursive result.
- Stop when `n == 0`.
- Each recursive call reduces the number of digits by one.

**Your solution is correct and optimal for this recursive approach.**
