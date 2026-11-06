# Combination Sum — LeetCode 39

**Pattern:** Recursion + Backtracking (Pick / Not Pick)

## 1. Is your code correct?

<badge color="success">Correct</badge>

Your code is correct. You are using the **Pick / Not Pick approach**.

The most important part is:

```cpp
// Not Pick
func(candidates, n, target, sum, i+1, temp, ans);

// Pick
temp.push_back(candidates[i]);
sum += candidates[i];

func(candidates, n, target, sum, i, temp, ans);

temp.pop_back();
sum -= candidates[i];
```

Notice the difference:

- **Not Pick:** `i + 1` because we move to the next candidate.
- **Pick:** `i` because we can reuse the same candidate unlimited times.

This is exactly what the problem requires.

## 2. Approach

At every index, we have two choices:

**Choice 1: Not Pick**

Skip the current candidate and move to the next index.

```cpp
func(candidates, n, target, sum, i+1, temp, ans);
```

**Choice 2: Pick**

Select the current candidate and stay at the same index because we can use it again.

```cpp
temp.push_back(candidates[i]);
sum += candidates[i];

func(candidates, n, target, sum, i, temp, ans);
```

After the recursive call, undo the selection:

```cpp
temp.pop_back();
sum -= candidates[i];
```

**Base cases:**

```cpp
if(sum == target) {
    ans.push_back(temp);
    return;
}

if(i == n || sum > target) {
    return;
}
```

- If `sum == target`, we found a valid combination.
- If `i == n`, all candidates have been considered.
- If `sum > target`, the current combination cannot become valid because all candidates are positive.

## 3. Your Code

Your implementation is correct. Here is the same code with slightly cleaner formatting:

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates,
                                       int target) {

        vector<int> temp;
        vector<vector<int>> ans;

        int n = candidates.size();
        int sum = 0;

        func(candidates, n, target, sum, 0, temp, ans);

        return ans;
    }

    void func(vector<int>& candidates, int n, int target,
              int sum, int i, vector<int>& temp,
              vector<vector<int>>& ans) {

        if(sum == target) {
            ans.push_back(temp);
            return;
        }

        if(i == n || sum > target) {
            return;
        }

        // Not Pick
        func(candidates, n, target, sum, i+1, temp, ans);

        // Pick
        if(candidates[i] + sum <= target) {

            temp.push_back(candidates[i]);
            sum += candidates[i];

            func(candidates, n, target, sum, i, temp, ans);

            temp.pop_back();
            sum -= candidates[i];
        }
    }
};
```

## 4. Dry Run (candidates = [2, 3], target = 7)

Initially:

```cpp
func(candidates, 2, 7, 0, 0, temp, ans);
```

We have two candidates: `2` and `3`.

The recursion explores:

```text
                         sum = 0
                        /       \
                  Not Pick      Pick 2
                     |             |
                   sum=0         sum=2
                     |           /    \
                 Pick 3      Not Pick  Pick 2
                     |           |        |
                   sum=3       sum=2    sum=4
                     |                    |
                  Pick 3                Pick 2
                     |                    |
                   sum=6                sum=6
                     |                    |
                  Pick 3                Pick 2
                     |                    |
                   sum=9                sum=8
                 (Invalid)            (Invalid)
```

Let's trace the successful path:

```text
Pick 2 → Pick 2 → Pick 2 → Not Pick 2 → Pick 3
```

The sum becomes:

```text
2 + 2 + 3 = 7
```

So we store:

```cpp
[2, 2, 3]
```

Another successful path:

```text
Not Pick 2 → Not Pick 3
```

Actually, for the complete example `[2,3,6,7]`, the other valid combination is `[7]`, obtained by selecting `7` directly.

For the smaller input `[2,3]`, the only valid combination is:

```cpp
[[2,2,3]]
```

## 5. Why do we pass `i` when picking?

This is the most important concept in this problem.

Suppose:

```text
candidates = [2, 3, 6, 7]
```

When we pick `2`, we call:

```cpp
func(..., i, ...);
```

The index remains `0`, so we can pick `2` again.

For example:

```text
Pick 2 → Pick 2 → Pick 2
```

But when we don't pick `2`, we call:

```cpp
func(..., i+1, ...);
```

Now we move to candidate `3` and cannot return to `2`.

This also prevents duplicate combinations such as `[2,3]` and `[3,2]`.

We generate combinations in candidate-index order rather than generating every possible arrangement.

## 6. Complexity Analysis

Let:

- `N` = number of candidates.
- `T` = target.
- `M` = smallest candidate value.

Because the same candidate can be selected repeatedly, recursion depth can be at most \(T/M\).

- **Time Complexity:** Exponential in the worst case; a safe upper bound is \(O(2^{N+T/M})\), excluding the cost of copying stored combinations.
- **Auxiliary Space:** \(O(N + T/M)\) for recursion and the temporary combination.
- **Output Space:** Depends on the number and size of valid combinations.

## 7. Key Takeaways

| Situation | Recursive index |
|---|---|
| Not Pick | `i + 1` |
| Pick | `i` |

1. This is a Pick / Not Pick recursion problem.
2. Use `i` when unlimited reuse is allowed.
3. Use `i + 1` when skipping a candidate.
4. `temp` stores the current combination.
5. `pop_back()` performs backtracking.
6. Since all candidates are positive, `sum > target` is a valid pruning condition.
