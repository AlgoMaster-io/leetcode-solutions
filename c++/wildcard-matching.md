# [LeetCode Problem 44: Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)

## Table of Contents
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Top-Down Dynamic Programming (Memoization)](#approach-2-top-down-dynamic-programming-memoization)
- [Approach 3: Bottom-Up Dynamic Programming (Tabulation)](#approach-3-bottom-up-dynamic-programming-tabulation)
- [Approach 4: Two-Pointer Technique](#approach-4-two-pointer-technique)

## Approach 1: Recursive Approach

### Intuition:
This approach uses the classic recursive backtracking technique to match the string with the pattern. The idea is to try and match one character at a time using recursion. If either a match is found or a wildcard character `*` is encountered, recursive calls are made accordingly.

```cpp
bool isMatchHelper(const std::string& s, const std::string& p, int sIndex, int pIndex) {
    // Base case: If both string and pattern reach the end
    if (pIndex == p.length()) return sIndex == s.length();
    
    // Check current characters match or pattern has '?' wildcard
    bool currentMatch = (sIndex < s.length() &&
                        (p[pIndex] == s[sIndex] || p[pIndex] == '?'));
    
    // If pattern is '*', we have two choices: 
    // 1. Consider '*' as an empty sequence.
    // 2. Consider '*' as taking one character from the string.
    if (p[pIndex] == '*') {
        return (isMatchHelper(s, p, sIndex, pIndex + 1) || // '*' matches nothing
                (sIndex < s.length() && isMatchHelper(s, p, sIndex + 1, pIndex))); // '*' matches one char
    } else {
        return currentMatch && isMatchHelper(s, p, sIndex + 1, pIndex + 1);
    }
}

bool isMatch(const std::string& s, const std::string& p) {
    return isMatchHelper(s, p, 0, 0);
}
```

#### Time Complexity:
- Exponential in the worst-case since it checks all possible matches.

#### Space Complexity:
- Stack space complexity can be O(n + m) due to recursion, where n is the length of the string and m is the length of the pattern.

## Approach 2: Top-Down Dynamic Programming (Memoization)

### Intuition:
To optimize the naive recursion, use a memoization table to store results of subproblems, avoiding redundant computations.

```cpp
bool isMatchHelperMemo(const std::string& s, const std::string& p, int sIndex, int pIndex, std::vector<std::vector<int>>& memo) {
    // Use a memoization table to store results of subproblems
    if (memo[sIndex][pIndex] != -1) return memo[sIndex][pIndex];
    
    if (pIndex == p.length()) return memo[sIndex][pIndex] = (sIndex == s.length());
    
    bool currentMatch = (sIndex < s.length() &&
                        (p[pIndex] == s[sIndex] || p[pIndex] == '?'));
    
    if (p[pIndex] == '*') {
        memo[sIndex][pIndex] = (isMatchHelperMemo(s, p, sIndex, pIndex + 1, memo) || 
                               (sIndex < s.length() && isMatchHelperMemo(s, p, sIndex + 1, pIndex, memo)));
    } else {
        memo[sIndex][pIndex] = currentMatch && isMatchHelperMemo(s, p, sIndex + 1, pIndex + 1, memo);
    }
    
    return memo[sIndex][pIndex];
}

bool isMatch(const std::string& s, const std::string& p) {
    std::vector<std::vector<int>> memo(s.length() + 1, std::vector<int>(p.length() + 1, -1));
    return isMatchHelperMemo(s, p, 0, 0, memo);
}
```

#### Time Complexity:
- O(n * m), where n is the length of the string and m is the length of the pattern.

#### Space Complexity:
- O(n * m), for the memoization table, plus the recursion stack space.

## Approach 3: Bottom-Up Dynamic Programming (Tabulation)

### Intuition:
Convert the recursive solution to an iterative one using a 2D DP table, where `dp[i][j]` means whether `s[0..i-1]` matches `p[0..j-1]`.

```cpp
bool isMatch(const std::string& s, const std::string& p) {
    int n = s.length(), m = p.length();
    std::vector<std::vector<bool>> dp(n + 1, std::vector<bool>(m + 1, false));
    
    // Empty pattern matches empty string
    dp[0][0] = true;
    
    // Handle cases where pattern consists of '*' only
    for (int j = 1; j <= m; ++j) {
        if (p[j - 1] == '*') {
            dp[0][j] = dp[0][j - 1];
        }
    }
    
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            if (p[j - 1] == '*') {
                dp[i][j] = dp[i][j - 1] || dp[i - 1][j];
            } else if (p[j - 1] == '?' || s[i - 1] == p[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1];
            }
        }
    }
    
    return dp[n][m];
}
```

#### Time Complexity:
- O(n * m), where n is the length of the string and m is the length of the pattern.

#### Space Complexity:
- O(n * m), for the DP table.

## Approach 4: Two-Pointer Technique

### Intuition:
An optimized approach that leverages two pointers to traverse the string and pattern, along with backtracking to handle the '*' wildcard effectively.

```cpp
bool isMatch(const std::string& s, const std::string& p) {
    int sIndex = 0, pIndex = 0;
    int starIndex = -1, sTmp = -1;
    
    while (sIndex < s.length()) {
        if (pIndex < p.length() && (p[pIndex] == s[sIndex] || p[pIndex] == '?')) {
            ++sIndex;
            ++pIndex;
        } else if (pIndex < p.length() && p[pIndex] == '*') {
            starIndex = pIndex;
            sTmp = sIndex;
            ++pIndex;
        } else if (starIndex != -1) {
            pIndex = starIndex + 1;
            sIndex = sTmp + 1;
            sTmp = sIndex;
        } else {
            return false;
        }
    }
    
    while (pIndex < p.length() && p[pIndex] == '*') {
        ++pIndex;
    }
    
    return pIndex == p.length();
}
```

#### Time Complexity:
- O(n + m), where n is the length of the string and m is the length of the pattern. This is because each character in the string and pattern is processed at most once.

#### Space Complexity:
- O(1), as no extra space proportional to input size is used.

