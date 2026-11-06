# Generate Parentheses — LeetCode 22

**Pattern:** Recursion + Backtracking

## 1. Is your code correct?

**Yes! Your logic is correct.** You are using the right backtracking conditions.

There is just one improvement: your `func()` does not need to return `vector<string>`. It only modifies `ans`, which is already passed by reference. So, make its return type `void`.

## 2. Approach

We maintain three things:

- `open`: Number of opening brackets `(` used.
- `close`: Number of closing brackets `)` used.
- `s`: Current combination.

There are two conditions:

**Condition 1: Add an opening bracket**

```cpp
if(open < n)
```

We can add `(` as long as we haven't used all `n` opening brackets.

**Condition 2: Add a closing bracket**

```cpp
if(close < open)
```

We can add `)` only when the number of closing brackets is less than the number of opening brackets.

This ensures that every prefix of the string is valid.

**Base case:**

```cpp
if(s.size() == 2*n)
```

When the string contains `2*n` characters, we have a complete valid combination. Add it to `ans` and return.

## 3. Corrected Code

```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        func(0, 0, "", n, ans);
        return ans;
    }

    void func(int open, int close, string s, int n,
              vector<string>& ans) {

        if(s.size() == 2*n) {
            ans.push_back(s);
            return;
        }

        if(open < n) {
            func(open + 1, close, s + "(", n, ans);
        }

        if(close < open) {
            func(open, close + 1, s + ")", n, ans);
        }
    }
};
```

## 4. Dry Run (n = 2)

Initially:

```cpp
func(0, 0, "", 2, ans);
```

The recursion explores these branches:

```text
                         ""
                         |
                        "("
                       /   \
                    "(("   "()"
                     |       |
                   "(()"   "()("
                     |       |
                  "(())"   "()()"
```

The valid combinations are:

```cpp
["(())", "()()"]
```

For `n = 3`, the output is:

```cpp
["((()))", "(()())", "(())()", "()(())", "()()()"]
```

## 5. Why does `close < open` work?

Consider this invalid combination:

```text
())(
```

After adding the first `(`:

- `open = 1`
- `close = 0`

We can add `)` because `close < open`.

After adding another `)`:

- `open = 1`
- `close = 2`

Now `close < open` is false, so recursion cannot add another closing bracket or continue with an invalid prefix.

This condition prevents invalid combinations from ever being generated.

## 6. Complexity Analysis

Let \(C_n\) be the nth Catalan number.

- **Time Complexity:** \(O(n \cdot C_n)\), because there are \(C_n\) valid strings, each of length \(2n\).
- **Auxiliary Space:** \(O(n)\) recursion depth, excluding the output.
- **Output Space:** \(O(n \cdot C_n)\).

## 7. Key Takeaways

1. This is a classic **Backtracking** problem.
2. We don't need to generate all possible strings and then check validity.
3. Instead, we generate only valid combinations.
4. `open < n` controls opening brackets.
5. `close < open` controls closing brackets.
6. Always return after reaching the base case.
