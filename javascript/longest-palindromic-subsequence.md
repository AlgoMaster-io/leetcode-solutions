# [Leetcode 516: Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Approaches
- [Approach 1: Recursion with Memoization](#approach-1-recursion-with-memoization)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

## Approach 1: Recursion with Memoization

### Intuition
The problem asks us to find the longest palindromic subsequence in a given string. By using a recursive approach, we can break down the problem into smaller subproblems. If the characters at the current bounds are the same, they can be part of the longest palindromic subsequence. Otherwise, we check which subsequence is longer when excluding either the left or the right character. Using memoization will help us store already computed results for certain indices to avoid redundant calculations and improve efficiency.

### Steps
1. Define a helper recursive function `findLPS(left, right)`.
2. If `left > right`, return 0, because we've examined all possibilities.
3. If `left == right`, return 1, meaning that a single character is a palindrome itself.
4. If `s[left] == s[right]`, we move both pointers inward: `2 + findLPS(left + 1, right - 1)`.
5. Otherwise, explore two possibilities: move `left` pointer or `right` pointer: `max(findLPS(left + 1, right), findLPS(left, right - 1))`.
6. Store results in a memoization table to avoid recalculating values.

### Code

```javascript
var longestPalindromeSubseq = function(s) {
    const memo = Array(s.length).fill(null).map(() => Array(s.length).fill(-1));

    function findLPS(left, right) {
        // Base case: invalid boundaries
        if (left > right) return 0;
        
        // Base case: single character
        if (left === right) return 1;
        
        // Return memoized result
        if (memo[left][right] !== -1) return memo[left][right];
        
        // If characters are the same, include them and move inward
        if (s[left] === s[right]) {
            memo[left][right] = 2 + findLPS(left + 1, right - 1);
        } else {
            // Otherwise, check both possibilities
            memo[left][right] = Math.max(findLPS(left + 1, right), findLPS(left, right - 1));
        }
        
        return memo[left][right];
    }
    
    return findLPS(0, s.length - 1);
};
```

### Complexity
- **Time Complexity**: O(n^2), where n is the length of the string. Each substring possibility is calculated once and stored.
- **Space Complexity**: O(n^2) due to the memoization table.

## Approach 2: Dynamic Programming

### Intuition
Dynamic programming can be used to solve this problem in a bottom-up manner. We can fill a 2D table where `dp[i][j]` represents the longest palindromic subsequence in `s[i...j]`. The main idea is that if the characters `s[i]` and `s[j]` are the same, they can potentially form part of the subsequence, thus extending the length by 2 plus whatever the subsequence within those indices would yield. Otherwise, take the maximum result by excluding either end of the current substring.

### Steps
1. Create a 2D DP table where `dp[i][j]` represents the longest palindromic subsequence between indices `i` and `j`.
2. Start filling the table from the base case where every individual character is a palindrome of length 1.
3. For lengths greater than 1, fill the DP table using the earlier explained intuition.
4. Finally, return the value at `dp[0][n-1]`.

### Code

```javascript
var longestPalindromeSubseq = function(s) {
    const n = s.length;
    const dp = Array.from({length: n}, () => Array(n).fill(0));
    
    // Single-character palindromes
    for (let i = 0; i < n; i++) {
        dp[i][i] = 1;
    }
    
    // Consider all substrings of length 2 to n
    for (let len = 2; len <= n; len++) {
        for (let i = 0; i < n - len + 1; i++) {
            let j = i + len - 1;
            if (s[i] === s[j]) {
                dp[i][j] = 2 + (i + 1 <= j - 1 ? dp[i + 1][j - 1] : 0);
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    
    return dp[0][n - 1];
};
```

### Complexity
- **Time Complexity**: O(n^2), with n being the length of the string, due to iterating over all substrings.
- **Space Complexity**: O(n^2) for the DP table.

