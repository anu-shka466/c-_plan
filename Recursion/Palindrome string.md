# Palindrome String — GFG

## Pattern
Recursion + Two Pointers

## Approach

Your solution uses recursion to compare characters from both ends of the string.

We maintain two pointers:

- `l` starts from index `0`.
- `h` starts from the last index.

### Step 1: Base Case

```cpp
if(l >= h) return true;
```

When both pointers meet or cross each other, all characters have matched.

Therefore, the string is a palindrome.

### Step 2: Compare Characters

```cpp
if(s.at(l) != s.at(h)) return false;
```

If the characters are different, the string cannot be a palindrome.

### Step 3: Recursive Call

```cpp
return func(s, l+1, h-1);
```

If both characters match:
- Move `l` one step forward.
- Move `h` one step backward.
- Repeat the comparison.

## Code (C++)

```cpp
class Solution {
public:
    bool isPalindrome(string& s) {
        int l = 0;
        int h = s.size() - 1;

        return func(s, l, h);
    }

    bool func(string& s, int l, int h) {

        // Base case
        if(l >= h) return true;

        // Character mismatch
        if(s.at(l) != s.at(h)) return false;

        // Recursive call
        return func(s, l + 1, h - 1);
    }
};
```

Your implementation is correct.

## Dry Run 1

**Input:**

```cpp
s = "abba";
```

Initially:

```text
l = 0, h = 3

s[l] = 'a'
s[h] = 'a'
```

Characters match.

Recursive call:

```cpp
func(s, 1, 2);
```

Second comparison:

```text
l = 1, h = 2

s[l] = 'b'
s[h] = 'b'
```

Characters match.

Recursive call:

```cpp
func(s, 2, 1);
```

Now:

```cpp
l >= h
```

Base case returns `true`.

**Output:**

```cpp
true
```

## Dry Run 2

**Input:**

```cpp
s = "abc";
```

Initially:

```text
l = 0, h = 2

s[l] = 'a'
s[h] = 'c'
```

Characters don't match.

Therefore:

```cpp
return false;
```

**Output:**

```cpp
false
```

## Complexity Analysis

Let `n` be the length of the string.

- **Time Complexity:** `O(n)` — We compare approximately `n/2` pairs of characters.
- **Auxiliary Space Complexity:** `O(n)` — Recursive calls use the call stack, with approximately `n/2` recursive calls.

## Important C++ Note

Your declaration:

```cpp
int h = s.size() - 1;
```

works for the given non-empty strings.

However, for an empty string, `s.size()` is unsigned, so subtracting `1` can cause an underflow before conversion to `int`.

A safer initialization is:

```cpp
int h = (int)s.size() - 1;
```

Your base case will then correctly handle an empty string.

## Key Takeaways

- Compare the first and last characters.
- If they match, move both pointers inward.
- If they don't match, return `false`.
- Stop when `l >= h`.
- Recursion avoids writing an explicit loop, but uses `O(n)` call-stack space.

**Your recursive approach is correct.**
