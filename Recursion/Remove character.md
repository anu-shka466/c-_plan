# Remove Character — GFG

## Pattern
Recursion + String Traversal

## Is your code correct?

<badge color="success">Correct output</badge>

Your logic is correct. You are using an additional string `ans` to store characters that are not equal to `c`.

However, there is one unnecessary thing in your code: your recursive function has return type `string`, but you are not actually returning or using any meaningful string.

You can change its return type to `void`.

## Approach

We solve this problem using recursion.

1. Start traversing the string from index `0`.
2. Check whether the current character is equal to `c`.
3. If it is not equal, add it to `ans`.
4. Recursively process the next character.
5. When `i == n`, stop the recursion.
6. Finally, assign `ans` back to `s`.

## Improved Code (C++)

```cpp
class Solution {
public:
    void removeCharacter(string &s, char c) {

        string ans;
        int n = s.size();

        func(s, n, c, 0, ans);

        s = ans;
    }

    void func(string &s, int n, char c, int i, string &ans) {

        // Base case
        if(i == n) return;

        // Keep character if it is not equal to c
        if(s[i] != c) {
            ans.push_back(s[i]);
        }

        // Recursive call
        func(s, n, c, i + 1, ans);
    }
};
```

## Dry Run

**Input:**

```cpp
s = "geeksforgeeks";
c = 'e';
```

Initially:

```cpp
ans = "";
i = 0;
```

| Index | Character | Condition | ans |
|---|---|---|---|
| 0 | g | Keep | g |
| 1 | e | Remove | g |
| 2 | e | Remove | g |
| 3 | k | Keep | gk |
| 4 | s | Keep | gks |
| 5 | f | Keep | gksf |
| 6 | o | Keep | gksfo |
| 7 | r | Keep | gksfor |
| 8 | g | Keep | gksforg |
| 9 | e | Remove | gksforg |
| 10 | e | Remove | gksforg |
| 11 | k | Keep | gksforgk |
| 12 | s | Keep | gksforgks |

When `i == 13`, the base case executes.

Finally:

```cpp
s = ans;
```

**Output:**

```cpp
"gksforgks"
```

## Complexity Analysis

Let `n` be the length of the string.

- **Time Complexity:** `O(n)` — Every character is visited exactly once.
- **Auxiliary Space Complexity:** `O(n)` — The answer string stores the remaining characters, and recursion uses `O(n)` call-stack space.

## Can we optimize it?

Yes. We can modify the original string using a recursive approach without creating another string:

```cpp
class Solution {
public:
    void removeCharacter(string &s, char c) {
        func(s, c, 0, 0);
    }

    void func(string &s, char c, int i, int j) {

        if(i == s.size()) {
            s.resize(j);
            return;
        }

        if(s[i] != c) {
            s[j] = s[i];
            j++;
        }

        func(s, c, i + 1, j);
    }
};
```

Here:
- `i` is the index used to read characters.
- `j` is the index where the next valid character is written.
- `resize(j)` removes the unused characters at the end.

This version uses `O(1)` auxiliary space apart from the recursive call stack, which still takes `O(n)`.

## Key Takeaways

- Your original approach is correct.
- Change the recursive function's return type from `string` to `void`.
- Pass `ans` by reference to avoid copying it during recursive calls.
- The second approach avoids creating an additional string.
- Recursion still requires `O(n)` stack space.
