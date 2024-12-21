# [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

## Table of Contents
- [Approach 1: Backtracking](#approach-1-backtracking)
- [Approach 2: Dynamic Programming + Backtracking](#approach-2-dynamic-programming--backtracking)

### Approach 1: Backtracking

Intuition:
The core idea is to use backtracking to generate all possible partitioning of the string, and for each partitioning, check if the substrings are palindromes. Backtracking is well-suited for this type of problem because we can recursively try each possible partition and backtrack if it does not lead to a solution.

1. Start from the beginning of the string and for each character at position `i`, check if the substring from the start to `i` is a palindrome.
2. If it is a palindrome, recursively partition the rest of the string from `i+1`.
3. If a partitioning of the entire string is found, add it to the result list.

```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> result = new ArrayList<>();
        backtrack(s, 0, new ArrayList<>(), result);
        return result;
    }
    
    private void backtrack(String s, int start, List<String> current, List<List<String>> result) {
        // Base case: If we've reached the end of the string, add the current partition to result
        if (start == s.length()) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        // Try to partition at each possible position
        for (int end = start; end < s.length(); end++) {
            // Check if the substring s[start:end+1] is a palindrome
            if (isPalindrome(s, start, end)) {
                current.add(s.substring(start, end + 1)); // Make a choice
                backtrack(s, end + 1, current, result); // Explore
                current.remove(current.size() - 1); // Undo the choice
            }
        }
    }
    
    private boolean isPalindrome(String s, int left, int right) {
        // Check if the string between left and right is a palindrome
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```

**Time Complexity:** O(n * 2^n), where n is the length of the string. This complexity is due to the fact that each character might either start its own substring or extend a previous one, leading to exponential possibilities.

**Space Complexity:** O(n), where n is the depth of the recursion. The space is used by the recursive call stack.

### Approach 2: Dynamic Programming + Backtracking

Intuition:
We can optimize the palindrome checking part by using dynamic programming (DP). Using a 2D DP array, we can store the palindrome status of substrings to avoid recomputing this information repeatedly.

1. Precompute a 2D array `dp` where `dp[i][j]` is true if the substring `s[i:j+1]` is a palindrome.
2. Use this DP table in the backtracking process to quickly check if a substring is a palindrome.

```java
class Solution {
    public List<List<String>> partition(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        // Precompute whether s[i, j] is a palindrome
        for (int length = 1; length <= n; length++) {
            for (int i = 0; i <= n - length; i++) {
                int j = i + length - 1;
                if (s.charAt(i) == s.charAt(j) && (length <= 2 || dp[i+1][j-1])) {
                    dp[i][j] = true;
                }
            }
        }
        
        List<List<String>> result = new ArrayList<>();
        backtrack(s, 0, new ArrayList<>(), result, dp);
        return result;
    }
    
    private void backtrack(String s, int start, List<String> current, List<List<String>> result, boolean[][] dp) {
        if (start == s.length()) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        for (int end = start; end < s.length(); end++) {
            // Use precomputed DP table to check for palindrome
            if (dp[start][end]) {
                current.add(s.substring(start, end + 1));
                backtrack(s, end + 1, current, result, dp);
                current.remove(current.size() - 1);
            }
        }
    }
}
```

**Time Complexity:** O(n^2 * 2^n), where n is the length of the string. The DP precomputation is O(n^2), and the backtracking process remains exponential.

**Space Complexity:** O(n^2) for the DP table plus O(n) for the recursion stack.

