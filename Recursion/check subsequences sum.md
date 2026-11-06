# Check Subsequence Sum — Core

**Pattern:** Recursion + Backtracking (Pick / Not Pick)

## 1. Is your code correct?

<badge color="success">Correct</badge>

Your Java code is logically correct. It checks whether at least one subsequence has a sum equal to `k`.

Since your current DSA practice is in C++, here is the equivalent C++ implementation.

## 2. Approach

For every element, we have two choices:

1. **Pick:** Include the current element in the subsequence.
2. **Not Pick:** Skip the current element.

We recursively explore both possibilities.

We maintain:

- `ind`: Current index.
- `sum`: Sum of selected elements.
- `nums`: Input array.
- `k`: Required sum.

**Base case:**

```cpp
if(ind == nums.size()) {
    return sum == k;
}
```

When we have processed all elements, check whether the selected elements sum to `k`.

## 3. C++ Code

```cpp
class Solution {
public:
    bool checkSubsequenceSum(vector<int>& nums, int k) {
        return func(0, 0, nums, k);
    }

    bool func(int ind, int sum, vector<int>& nums, int k) {

        if(ind == nums.size()) {
            return sum == k;
        }

        // Pick the current element
        if(func(ind + 1, sum + nums[ind], nums, k)) {
            return true;
        }

        // Not Pick the current element
        if(func(ind + 1, sum, nums, k)) {
            return true;
        }

        return false;
    }
};
```

## 4. Dry Run

Input:

```cpp
nums = [1, 2, 3, 4, 5]
k = 8
```

One successful path is:

```text
                         []
                       sum = 0
                          |
                       Pick 1
                          |
                       sum = 1
                          |
                       Pick 2
                          |
                       sum = 3
                          |
                      Not Pick 3
                          |
                       sum = 3
                          |
                      Not Pick 4
                          |
                       sum = 3
                          |
                       Pick 5
                          |
                       sum = 8
                          |
                     Return true
```

The selected subsequence is:

```cpp
[1, 2, 5]
```

Its sum is:

```text
1 + 2 + 5 = 8
```

Therefore, the function returns:

```cpp
true
```

## 5. Why do we return `true` immediately?

Consider:

```cpp
if(func(ind + 1, sum + nums[ind], nums, k)) {
    return true;
}
```

If the Pick branch finds a valid subsequence, there is no need to explore the Not Pick branch.

We only need to determine whether **at least one** valid subsequence exists.

This is called short-circuiting the recursion.

## 6. Complexity Analysis

For every element, we have two choices: Pick or Not Pick.

- **Time Complexity:** \(O(2^n)\), because there are at most \(2^n\) subsequences.
- **Auxiliary Space:** \(O(n)\), due to recursion depth.
- **Output Space:** \(O(1)\), because we only return a boolean and do not store subsequences.

## 7. Important Interview Note

Your base case checks the sum only when all elements have been processed.

If all array elements are **non-negative**, you can optimize by adding:

```cpp
if(sum > k) return false;
```

This is valid only when all remaining elements are non-negative. If negative numbers are allowed, the sum could decrease later, so this pruning would be incorrect.

## 8. Key Takeaways

1. Every element has two choices: Pick or Not Pick.
2. `ind + 1` is used in both recursive calls because each element is considered only once.
3. The base case checks whether the final sum equals `k`.
4. Return `true` as soon as any branch finds a valid subsequence.
5. This is a fundamental Pick / Not Pick recursion pattern.
