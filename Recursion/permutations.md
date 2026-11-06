# Permutations — LeetCode 46

**Pattern:** Recursion + Backtracking (Swapping)

## 1. Is your code correct?

<badge color="success">Correct</badge>

Your code is completely correct. You are using the **swap-based backtracking approach**.

The most important part of your solution is:

```cpp
swap(nums[ind], nums[i]);
func(nums, n, ind + 1, ans);
swap(nums[ind], nums[i]);
```

- First `swap()` selects an element for the current position.
- Recursive call fills the remaining positions.
- Second `swap()` restores the original array so that we can explore other possibilities.

## 2. Approach

We use `ind` to represent the position that we are currently filling.

For every position:

1. Try every element from index `ind` to `n-1`.
2. Swap the current element with `nums[ind]`.
3. Recursively generate permutations for the remaining positions.
4. Swap back to restore the original arrangement.

**Base case:**

```cpp
if(ind == n) {
    ans.push_back(nums);
    return;
}
```

When `ind == n`, every position has been fixed, so we have one complete permutation.

## 3. Code

```cpp
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {

        vector<vector<int>> ans;
        int n = nums.size();

        func(nums, n, 0, ans);

        return ans;
    }

    void func(vector<int>& nums, int n, int ind,
              vector<vector<int>>& ans) {

        if(ind == n) {
            ans.push_back(nums);
            return;
        }

        for(int i = ind; i < n; i++) {

            swap(nums[ind], nums[i]);

            func(nums, n, ind + 1, ans);

            swap(nums[ind], nums[i]);
        }
    }
};
```

## 4. Dry Run (nums = [1, 2, 3])

Initially:

```text
nums = [1, 2, 3]
ind = 0
```

**Recursion tree:**

```text
                         [1,2,3]
                       /    |    \
                      /     |     \
                 [1,2,3] [2,1,3] [3,2,1]
                    |       |       |
                    |       |       |
                 [1,2,3] [2,1,3] [3,2,1]
                  /   \    /   \    /   \
             [1,2,3] [1,3,2] [2,1,3] [2,3,1] [3,2,1] [3,1,2]
```

The six permutations generated are:

```cpp
[
    [1,2,3],
    [1,3,2],
    [2,1,3],
    [2,3,1],
    [3,2,1],
    [3,1,2]
]
```

## 5. Why do we swap back?

Let's understand this carefully.

Initially:

```text
nums = [1, 2, 3]
ind = 0
```

The first iteration:

```cpp
swap(nums[0], nums[0]);
```

Array remains:

```text
[1, 2, 3]
```

Recursive calls generate:

```text
[1, 2, 3]
[1, 3, 2]
```

After recursion finishes, we execute:

```cpp
swap(nums[0], nums[0]);
```

Now we can try the next element.

Second iteration:

```cpp
swap(nums[0], nums[1]);
```

Array becomes:

```text
[2, 1, 3]
```

Recursive calls generate:

```text
[2, 1, 3]
[2, 3, 1]
```

After recursion, we swap back:

```cpp
swap(nums[0], nums[1]);
```

Array becomes:

```text
[1, 2, 3]
```

This restoration is necessary because every new branch should start with the same original arrangement for that recursion level.

**Remember:**

```text
Choose → Explore → Undo
```

This is the fundamental idea of backtracking.

## 6. Complexity Analysis

For `n` distinct elements:

- **Number of permutations:** \(n!\)
- **Time Complexity:** \(O(n \times n!)\), because there are `n!` permutations and copying each permutation takes O(n).
- **Auxiliary Space:** \(O(n)\) for recursion depth.
- **Output Space:** \(O(n \times n!)\).

## 7. Key Takeaways

1. Each recursion level fixes one position.
2. The loop tries every available element for that position.
3. Swapping selects an element.
4. Recursive calls arrange the remaining elements.
5. Swapping back restores the array.
6. This approach does not require an additional `visited` array.
7. For distinct elements, this generates every permutation exactly once.
