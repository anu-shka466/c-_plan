# Letter Combinations of a Phone Number — LeetCode 17

**Pattern:** Recursion + Backtracking

## 1. Is your code correct?

**Yes! Your backtracking logic is correct.** You are generating all possible combinations by selecting one letter for each digit.

There are two improvements:

1. Handle an empty input string. Otherwise, your base case will push an empty string into `ans`.
2. Pass the `unordered_map` by reference instead of copying it in every recursive call.

## 2. Approach

We use an `unordered_map` to store the digit-to-letter mapping.

For example:

```cpp
2 -> "abc"
3 -> "def"
4 -> "ghi"
```

We maintain:

- `i`: Current digit index.
- `temp`: Current combination being generated.
- `ans`: Stores all completed combinations.

At every recursive call:

1. If `i == n`, we have selected one letter for every digit. Add `temp` to `ans`.
2. Get the letters corresponding to the current digit.
3. Iterate through those letters.
4. Add one letter to `temp`.
5. Recursively process the next digit.
6. Remove the last character using `pop_back()` to explore another choice.

## 3. Corrected Code

```cpp
class Solution {
public:
    vector<string> letterCombinations(string digits) {

        if(digits.empty()) return {};

        unordered_map<char, string> map;

        map['2'] = "abc";
        map['3'] = "def";
        map['4'] = "ghi";
        map['5'] = "jkl";
        map['6'] = "mno";
        map['7'] = "pqrs";
        map['8'] = "tuv";
        map['9'] = "wxyz";

        vector<string> ans;
        string temp = "";

        int n = digits.size();

        func(digits, n, 0, temp, ans, map);

        return ans;
    }

    void func(string &digits, int n, int i,
              string &temp, vector<string> &ans,
              unordered_map<char, string> &map) {

        if(i == n) {
            ans.push_back(temp);
            return;
        }

        string choice = map[digits[i]];

        for(int j = 0; j < choice.size(); j++) {

            temp.push_back(choice[j]);

            func(digits, n, i + 1, temp, ans, map);

            temp.pop_back();
        }
    }
};
```

## 4. Dry Run (digits = "23")

Mapping:

```text
2 -> abc
3 -> def
```

Initially:

```cpp
func("23", 2, 0, "", ans, map);
```

At index `0`, digit is `2`, so choices are `abc`.

For each choice, we move to index `1`, where digit is `3`.

<text weight="semibold">Recursion tree</text>

```text
                       ""
                  /     |     \
                 a      b      c
               / | \  / | \  / | \
              ad ae af bd be bf cd ce cf
```

The final answer is:

```cpp
["ad", "ae", "af",
 "bd", "be", "bf",
 "cd", "ce", "cf"]
```

## 5. Why do we use `pop_back()`?

Consider the first choice:

```cpp
temp.push_back('a');
```

Now:

```text
temp = "a"
```

The recursive call generates:

```text
ad
ae
af
```

After returning from recursion, we execute:

```cpp
temp.pop_back();
```

Now `temp` becomes empty again.

This is important because we need to explore the next choice, `b`, independently.

Without `pop_back()`, previously selected characters would remain in `temp`, and the combinations would be incorrect.

This process is called **backtracking: choose → explore → undo**.

## 6. Complexity Analysis

Let `n` be the number of digits.

- **Time Complexity:** \(O(4^n \cdot n)\) as an upper bound. Each digit can have at most 4 choices, and each completed combination has length `n`.
- **Auxiliary Space:** \(O(n)\) for recursion depth and the temporary string.
- **Output Space:** \(O(4^n \cdot n)\) in the worst case.

## 7. Key Takeaways

1. This is a classic recursion + backtracking problem.
2. Each recursive level represents one digit.
3. Each loop iteration represents one possible letter.
4. `push_back()` chooses a letter.
5. `pop_back()` undoes that choice.
6. The base case is reached when all digits have been processed.
7. Always handle empty input separately when the expected output is an empty list.
