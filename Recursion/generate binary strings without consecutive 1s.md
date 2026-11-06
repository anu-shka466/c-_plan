# Generate Binary Strings Without Consecutive 1s — Core

**Pattern:** Recursion + Backtracking (Choice-based Recursion)

## 1. Is your code correct?

<badge color="success">Correct</badge>

Your code is completely correct. It generates all binary strings of length `n` without consecutive `1`s.

It also generates them in **lexicographically increasing order** because you explore the `0` branch before the `1` branch.

## 2. Approach

At every position, we have two possible choices:

1. Add `'0'` — always allowed.
2. Add `'1'` — allowed only when the previous character is not `'1'`.

You maintain:

- `s`: Current binary string.
- `n`: Required string length.
- `prev`: Previous character.
- `ans`: Stores all valid strings.

**Base case:**

```cpp
if(s.size() == n) {
    ans.push_back(s);
    return;
}
```

When the string reaches length `n`, store it.

**Choice 1: Add 0**

```cpp
func(s + "0", n, '0', ans);
```

Zero can always be added. After adding it, the previous character becomes `'0'`.

**Choice 2: Add 1**

```cpp
if(prev != '1') {
    func(s + "1", n, '1', ans);
}
```

We add `1` only if the previous character is not `1`.

## 3. Code

Your code is correct. Here is the same implementation with clean formatting:

```cpp
class Solution {
public:
    vector<string> generateBinaryStrings(int n) {

        vector<string> ans;

        func("", n, '0', ans);

        return ans;
    }

    void func(string s, int n, char prev,
              vector<string>& ans) {

        if(s.size() == n) {
            ans.push_back(s);
            return;
        }

        // Always add 0
        func(s + "0", n, '0', ans);

        // Add 1 only if previous character is not 1
        if(prev != '1') {
            func(s + "1", n, '1', ans);
        }
    }
};
```

## 4. Dry Run (n = 3)

Initially:

```cpp
func("", 3, '0', ans);
```

Recursion tree:

```text
                         ""
                       /    \
                      0      1
                    /  \      |
                   00   01    10
                  / \    |    /  \
                000 001 010  100  101
```

Let's understand the branches:

- From `00`, we can add both `0` and `1`.
- From `01`, we can only add `0`.
- From `10`, we can add both `0` and `1`.
- We never add `1` after another `1`.

Final answer:

```cpp
["000", "001", "010", "100", "101"]
```

## 5. Why do we pass `prev`?

Consider:

```text
s = "01"
prev = '1'
```

We cannot add another `1`, because that would produce:

```text
"011"
```

which contains consecutive ones.

But we can add `0`:

```text
"010"
```

Now the previous character becomes `'0'`, so adding `1` at the next position is allowed.

That's why we update `prev` in every recursive call.

## 6. Why is the output lexicographically sorted?

You are exploring:

```cpp
func(s + "0", ...);
```

before:

```cpp
func(s + "1", ...);
```

Therefore, strings beginning with `0` are generated before strings beginning with `1`.

The same ordering is maintained at every position, so the final output is lexicographically increasing without needing `sort()`.

## 7. Complexity Analysis

Let `F(n)` be the number of valid binary strings of length `n`.

- **Time Complexity:** \(O(n \cdot F(n))\), including the cost of constructing and storing strings.
- **Auxiliary Space:** \(O(n)\) recursion depth, excluding stored strings.
- **Output Space:** \(O(n \cdot F(n))\).

The number of valid strings follows the Fibonacci sequence:

\[
F(n) = F(n-1) + F(n-2)
\]

with \(F(0)=1\) and \(F(1)=2\).

## 8. Key Takeaways

1. At every index, we have two choices: `0` or `1`.
2. `0` is always allowed.
3. `1` is allowed only when `prev != '1'`.
4. We update `prev` after every choice.
5. Exploring `0` before `1` automatically gives lexicographical order.
6. We don't need to generate invalid strings and check them later — we prevent them during recursion.
