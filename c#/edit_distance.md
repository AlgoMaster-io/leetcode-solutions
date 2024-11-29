# 72. [Edit Distance](https://leetcode.com/problems/edit-distance/)

## Approach 1: Recursive Solution

### Solution
csharp
```csharp
// Time Complexity: O(3^(m+n))
// Space Complexity: O(m+n)
public class Solution {
    public int MinDistance(string word1, string word2) {
        // Helper method to compute edit distance recursively
        return ComputeDistance(word1, word2, word1.Length, word2.Length);
    }

    private int ComputeDistance(string word1, string word2, int m, int n) {
        // If first string is empty, return length of second string
        if (m == 0) return n;
        
        // If second string is empty, return length of first string
        if (n == 0) return m;

        // If characters are the same, continue with next characters
        if (word1[m - 1] == word2[n - 1]) {
            return ComputeDistance(word1, word2, m - 1, n - 1);
        }

        // Calculate minimum operation from insert, remove, or replace
        return 1 + Math.Min(
            ComputeDistance(word1, word2, m, n - 1),    // Insert
            Math.Min(
                ComputeDistance(word1, word2, m - 1, n),    // Remove
                ComputeDistance(word1, word2, m - 1, n - 1) // Replace
            )
        );
    }
}
```

## Approach 2: Dynamic Programming (2D Table)

### Solution
csharp
```csharp
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
public class Solution {
    public int MinDistance(string word1, string word2) {
        int m = word1.Length;
        int n = word2.Length;
        
        // Create DP table
        int[,] dp = new int[m + 1, n + 1];

        // Initialize base cases
        for (int i = 0; i <= m; i++) {
            dp[i, 0] = i;
        }
        for (int j = 0; j <= n; j++) {
            dp[0, j] = j;
        }

        // Fill in DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i, j] = dp[i - 1, j - 1]; // Characters match, no new operation
                } else {
                    dp[i, j] = 1 + Math.Min(
                        dp[i - 1, j],    // Remove
                        Math.Min(
                            dp[i, j - 1],    // Insert
                            dp[i - 1, j - 1] // Replace
                        )
                    );
                }
            }
        }

        return dp[m, n];
    }
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
csharp
```csharp
// Time Complexity: O(m * n)
// Space Complexity: O(n)
public class Solution {
    public int MinDistance(string word1, string word2) {
        int m = word1.Length;
        int n = word2.Length;
        
        // Create two arrays for dynamic programming, only need two rows at a time
        int[] prev = new int[n + 1];
        int[] curr = new int[n + 1];

        // Initialize base case for the first row
        for (int j = 0; j <= n; j++) {
            prev[j] = j;
        }

        // Fill in DP table
        for (int i = 1; i <= m; i++) {
            curr[0] = i; // Base case for the first column
            for (int j = 1; j <= n; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    curr[j] = prev[j - 1]; // Characters match
                } else {
                    curr[j] = 1 + Math.Min(
                        prev[j],    // Remove
                        Math.Min(
                            curr[j - 1],    // Insert
                            prev[j - 1]     // Replace
                        )
                    );
                }
            }
            int[] temp = prev;
            prev = curr;
            curr = temp;
        }

        return prev[n];
    }
}
```

