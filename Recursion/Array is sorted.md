# Check if Array Is Sorted — GFG

## Pattern
Recursion + Array Traversal

## Approach

Your solution uses recursion to check whether the array is sorted in non-decreasing order.

An array is sorted in non-decreasing order if:

\[
arr[i] \leq arr[i+1]
\]

for every valid index `i`.

We check adjacent elements recursively.

### Step 1: Base Case

```cpp
if(i == arr.size() - 1) return true;
```

If we reach the last index, it means all adjacent elements have been checked successfully.

Therefore, return `true`.

### Step 2: Check Adjacent Elements

```cpp
if(arr[i] > arr[i+1]) return false;
```

If the current element is greater than the next element, the array is not sorted.

Return `false` immediately.

### Step 3: Recursive Call

```cpp
return sort(arr, i+1);
```

If the current pair is sorted, move to the next index.

## Code (C++)

Your logic is correct for non-empty arrays. Here is a slightly safer version that also handles an empty array:

```cpp
class Solution {
public:
    bool isSorted(vector<int>& arr) {
        return sort(arr, 0);
    }

    bool sort(vector<int>& arr, int i) {

        // Empty or single-element array
        if(i >= arr.size() - (arr.empty() ? 0 : 1))
            return true;

        // Check adjacent elements
        if(arr[i] > arr[i+1])
            return false;

        return sort(arr, i+1);
    }
};
```

A simpler implementation is to handle the empty array separately:

```cpp
class Solution {
public:
    bool isSorted(vector<int>& arr) {
        if(arr.size() <= 1) return true;

        return sort(arr, 0);
    }

    bool sort(vector<int>& arr, int i) {

        if(i == arr.size() - 1)
            return true;

        if(arr[i] > arr[i+1])
            return false;

        return sort(arr, i+1);
    }
};
```

## Dry Run 1

**Input:**

```cpp
arr = {10, 20, 30, 40, 50};
```

| Index | Comparison | Result |
|---|---|---|
| 0 | `10 > 20` | False |
| 1 | `20 > 30` | False |
| 2 | `30 > 40` | False |
| 3 | `40 > 50` | False |
| 4 | Base case | True |

All adjacent elements satisfy the condition.

**Output:**

```cpp
true
```

## Dry Run 2

**Input:**

```cpp
arr = {90, 80, 100, 70, 40, 30};
```

First comparison:

```cpp
arr[0] > arr[1]

90 > 80
```

Condition is true, so the function immediately returns `false`.

**Output:**

```cpp
false
```

## Complexity Analysis

Let `n` be the size of the array.

- **Time Complexity:** `O(n)` — In the worst case, we check every adjacent pair.
- **Auxiliary Space Complexity:** `O(n)` — Recursive call stack.

## Key Takeaways

- Non-decreasing means `arr[i] <= arr[i+1]`.
- Equal adjacent elements are allowed.
- Check one pair at a time.
- If any pair violates the condition, return `false`.
- If recursion reaches the last index, return `true`.
- Recursion uses `O(n)` stack space.

**Your recursive logic is correct. The only improvement needed is handling an empty array safely.**
