# Jump Game — LeetCode 55

**Pattern:** Greedy

## 1. Is your code correct?

<badge color="success">Your code is correct</badge>

Your greedy approach is optimal. You are maintaining the **maximum index reachable so far**.

No changes are required for the given problem constraints.

## 2. Understanding your approach

```cpp
int maxreach = 0;
```

`maxreach` stores the farthest index we can reach.

Now let's understand your loop:

**Condition 1:**
```cpp
if(i > maxreach) {
    return false;
}
```

If the current index is greater than `maxreach`, it means we cannot reach this index.

Therefore, reaching the last index is impossible.

**Condition 2:**
```cpp
maxreach = max(maxreach, i + nums[i]);
```

From the current index, we can jump at most `nums[i]` steps.

So the farthest index reachable from here is:

\[
i + nums[i]
\]

We update `maxreach` only if this new reach is greater.

**Condition 3:**
```cpp
if(maxreach >= nums.size() - 1)
    return true;
```

If the farthest reachable index is greater than or equal to the last index, we have reached our goal.

## 3. Dry run — Example 1

```cpp
nums = [2, 3, 1, 1, 4]
```

Initially:

```cpp
maxreach = 0;
```

| Index (i) | nums[i] | i + nums[i] | maxreach |
|---:|---:|---:|---:|
| 0 | 2 | 2 | 2 |
| 1 | 3 | 4 | 4 |

At index 1, `maxreach = 4`.

Last index = 4.

Therefore, return `true`.

<badge color="success">Answer = true</badge>

## 4. Dry run — Example 2

```cpp
nums = [3, 2, 1, 0, 4]
```

| Index (i) | nums[i] | i + nums[i] | maxreach |
|---:|---:|---:|---:|
| 0 | 3 | 3 | 3 |
| 1 | 2 | 3 | 3 |
| 2 | 1 | 3 | 3 |
| 3 | 0 | 3 | 3 |
| 4 | 4 | — | — |

At index 4:

```cpp
if(i > maxreach)
```

becomes:

```cpp
if(4 > 3)
```

This is true, so return `false`.

<badge color="danger">Answer = false</badge>

## 5. Your code

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxreach = 0;

        for(int i = 0; i < nums.size(); i++) {

            if(i > maxreach) {
                return false;
            }

            maxreach = max(maxreach, i + nums[i]);

            if(maxreach >= nums.size() - 1)
                return true;
        }

        return true;
    }
};
```

## 6. Complexity analysis

- **Time Complexity:** O(N) — Each index is visited at most once.
- **Auxiliary Space:** O(1) — Only one variable is maintained.

## 7. Important interview takeaway

The key idea is:

**We don't need to decide exactly how far to jump. We only need to track the farthest index we can reach.**

Remember:

- `i <= maxreach` → Current index is reachable.
- `i > maxreach` → Current index is unreachable.
- `maxreach >= n-1` → Last index is reachable.

This is the standard greedy solution for Jump Game.
