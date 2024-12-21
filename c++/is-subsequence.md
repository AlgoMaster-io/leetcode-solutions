[Leetcode 392: Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Approaches:
1. [Two Pointer Approach](#two-pointer-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)

### Two Pointer Approach

**Intuition:**

The two-pointer technique helps in scenarios where we need to find sequences or subsequences. For 'Is Subsequence', we can use two pointers to iterate over both the `s` and `t` strings. We check if all characters of `s` can be found in `t` sequentially.

**Steps:**
- Initialize two pointers `i` and `j` for strings `s` and `t` respectively.
- Move the pointer `i` in `s` forward only if the current character of `s` matches the current character of `t`.
- Always move the pointer `j` in `t` forward.
- If pointer `i` reaches the end of `s`, it means `s` is a subsequence of `t`.

**Code:**
```cpp
bool isSubsequence(string s, string t) {
    int i = 0, j = 0;
    // Iterate through the string t with the pointer j
    while (i < s.size() && j < t.size()) {
        // If characters match, move the i pointer in the string s
        if (s[i] == t[j]) {
            i++;
        }
        // Always move the j pointer in the string t
        j++;
    }
    // If i reached the end of s, all characters were matched
    return i == s.size();
}
```
**Complexity Analysis:**
- **Time Complexity:** O(n), where n is the length of the string `t`. We might need to traverse the whole string.  
- **Space Complexity:** O(1), no extra space is used except for the pointers.

---

### Dynamic Programming Approach

**Intuition:**

For a more generalized version, like when we have multiple `s` queries on the same `t`, we consider dynamic programming. We can precompute results to make each query faster.

**Steps:**
- Preprocess `t` and store the next occurrence of each character using a 2D array `dp` where `dp[i][c]` stores the next occurrence of character `c` after index `i`.
- Initialize `dp` with size `(m+1) x 26` where `m` is the length of `t` and `26` covers all lowercase characters.
- Fill `dp` from right to left, where `dp[m][c] = m` for all characters and `dp[i][c] = i` if `t[i] == c`, otherwise `dp[i][c] = dp[i+1][c]`.
- For each character of `s`, use the preprocessing step to quickly find its occurrence in `t`.

**Code:**
```cpp
bool isSubsequence(string s, string t) {
    int m = t.size();
    vector<vector<int>> dp(m + 1, vector<int>(26, m));
    
    // Fill the dp table backwards
    for (int i = m - 1; i >= 0; i--) {
        for (int c = 0; c < 26; c++) {
            dp[i][c] = dp[i + 1][c]; // Carry forward the next occurrence
        }
        dp[i][t[i] - 'a'] = i; // Update the current character position
    }
    
    int index = 0;
    for (char ch : s) {
        if (dp[index][ch - 'a'] == m) {
            return false; // Character not found
        }
        index = dp[index][ch - 'a'] + 1; // Move to next character's starting position
    }
    
    return true;
}
```

**Complexity Analysis:**
- **Time Complexity:** O(m + n), where `m` is the length of `t` and `n` is the length of `s`. Preprocessing takes `O(m)` and we scan `s` in `O(n)`.
- **Space Complexity:** O(m), for the DP table storing position indices.

