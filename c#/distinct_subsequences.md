# 115. [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Approach 1: Recursion with Memoization

### Solution
csharp
```csharp
// Time Complexity: O(n * m)
// Space Complexity: O(n * m)
public class Solution {
    public int NumDistinct(string s, string t) {
        // Memoization table
        int[,] memo = new int[s.Length, t.Length];
        // Initialize all entries with -1
        for (int i = 0; i < s.Length; i++) {
            for (int j = 0; j < t.Length; j++) {
                memo[i, j] = -1;
            }
        }
        return NumDistinctHelper(s, t, 0, 0, memo);
    }
    
    private int NumDistinctHelper(string s, string t, int sIndex, int tIndex, int[,] memo) {
        // If t is exhausted, one subsequence is found
        if (tIndex == t.Length) {
            return 1;
        }
        // If s is exhausted first, no subsequence can be formed
        if (sIndex == s.Length) {
            return 0;
        }
        // Check memoization table
        if (memo[sIndex, tIndex] != -1) {
            return memo[sIndex, tIndex];
        }
        
        // If characters match, both decisions can be made
        if (s[sIndex] == t[tIndex]) {
            memo[sIndex, tIndex] = NumDistinctHelper(s, t, sIndex + 1, tIndex + 1, memo) + 
                                   NumDistinctHelper(s, t, sIndex + 1, tIndex, memo);
        } else {
            // If characters don't match, skip the current character in s
            memo[sIndex, tIndex] = NumDistinctHelper(s, t, sIndex + 1, tIndex, memo);
        }
        
        return memo[sIndex, tIndex];
    }
}
```

## Approach 2: Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(n * m)
// Space Complexity: O(n * m)
public class Solution {
    public int NumDistinct(string s, string t) {
        // DP table
        int[,] dp = new int[s.Length + 1, t.Length + 1];

        // Base case: empty t can be formed by all possible substrings of s
        for (int i = 0; i <= s.Length; i++) {
            dp[i, t.Length] = 1;
        }

        // Fill the table in reverse order
        for (int i = s.Length - 1; i >= 0; i--) {
            for (int j = t.Length - 1; j >= 0; j--) {
                if (s[i] == t[j]) {
                    // Both using the character and ignoring it
                    dp[i, j] = dp[i + 1, j + 1] + dp[i + 1, j];
                } else {
                    // Ignore the character in s
                    dp[i, j] = dp[i + 1, j];
                }
            }
        }
        
        // Result is the number of ways to form t[0...m] from s[0...n]
        return dp[0, 0];
    }
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
csharp
```csharp
// Time Complexity: O(n * m)
// Space Complexity: O(m)
public class Solution {
    public int NumDistinct(string s, string t) {
        // Previous and current row for space optimization
        int[] prev = new int[t.Length + 1];
        int[] curr = new int[t.Length + 1];
        
        // Base case: empty t can be formed by all possible substrings of s
        for (int i = 0; i <= s.Length; i++) {
            prev[t.Length] = 1;
        }
        
        // Fill the table in reverse order
        for (int i = s.Length - 1; i >= 0; i--) {
            for (int j = t.Length - 1; j >= 0; j--) {
                if (s[i] == t[j]) {
                    curr[j] = prev[j + 1] + prev[j];
                } else {
                    curr[j] = prev[j];
                }
            }
            // Move current row to previous for next iteration
            Array.Copy(curr, prev, curr.Length);
        }
        
        // Result is the number of ways to form t[0...m] from s[0...n]
        return prev[0];
    }
}
```

