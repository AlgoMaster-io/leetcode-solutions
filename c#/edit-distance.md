# [Leetcode 72: Edit Distance](https://leetcode.com/problems/edit-distance/)

## Navigation
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Memoization (Top-Down Dynamic Programming)](#approach-2-memoization)
- [Approach 3: Bottom-Up Dynamic Programming](#approach-3-bottom-up-dynamic-programming)
- [Approach 4: Optimized Space Dynamic Programming](#approach-4-optimized-space-dynamic-programming)

## Approach 1: Recursive Approach

**Intuition:**

The naive recursive solution checks all possible actions (insert, delete, replace) at each step of transformation from one string to another. It explores all options recursively by writing a function to calculate the distance and iteratively adjusting the strings, choosing the minimal distance at each step.

This approach, while straightforward to implement, is highly inefficient due to overlapping subproblems and repeated calculations.

```csharp
public class Solution {
    private int MinDistanceRecursive(string word1, string word2, int i, int j) {
        // Base cases when one of the strings is exhausted
        if (i == word1.Length) return word2.Length - j;
        if (j == word2.Length) return word1.Length - i;
        
        // If characters are the same, move to the next character in both strings
        if (word1[i] == word2[j]) {
            return MinDistanceRecursive(word1, word2, i + 1, j + 1);
        } else {
            // If characters are different, consider all possibilities and choose the best minimum option
            int insertOp = 1 + MinDistanceRecursive(word1, word2, i, j + 1);
            int deleteOp = 1 + MinDistanceRecursive(word1, word2, i + 1, j);
            int replaceOp = 1 + MinDistanceRecursive(word1, word2, i + 1, j + 1);
            
            return Math.Min(insertOp, Math.Min(deleteOp, replaceOp));
        }
    }
    
    public int MinDistance(string word1, string word2) {
        return MinDistanceRecursive(word1, word2, 0, 0);
    }
}
```

**Complexity Analysis:**
- Time Complexity: \(O(3^{m+n})\), where `m` and `n` are the lengths of `word1` and `word2`.
- Space Complexity: \(O(m+n)\), due to the recursion stack.

---

## Approach 2: Memoization (Top-Down Dynamic Programming)

**Intuition:**

To solve the overlapping subproblem issue, we can store already solved states in a dictionary/memo table. This reduces the excessive recalculations present in the recursive approach. We modify the recursive approach by caching results and thus making it efficient.

```csharp
public class Solution {
    private int[,] memo;
    
    private int MinDistanceMemo(string word1, string word2, int i, int j) {
        if (i == word1.Length) return word2.Length - j;
        if (j == word2.Length) return word1.Length - i;
        
        if (memo[i, j] != -1) return memo[i, j];
            
        if (word1[i] == word2[j]) {
            memo[i, j] = MinDistanceMemo(word1, word2, i + 1, j + 1);
        } else {
            int insertOp = 1 + MinDistanceMemo(word1, word2, i, j + 1);
            int deleteOp = 1 + MinDistanceMemo(word1, word2, i + 1, j);
            int replaceOp = 1 + MinDistanceMemo(word1, word2, i + 1, j + 1);
            
            memo[i, j] = Math.Min(insertOp, Math.Min(deleteOp, replaceOp));
        }
        
        return memo[i, j];
    }
    
    public int MinDistance(string word1, string word2) {
        int m = word1.Length;
        int n = word2.Length;
        memo = new int[m, n];
        for (int k = 0; k < m; k++) for (int l = 0; l < n; l++) memo[k, l] = -1;
        
        return MinDistanceMemo(word1, word2, 0, 0);
    }
}
```

**Complexity Analysis:**
- Time Complexity: \(O(m \times n)\), as each state is solved only once.
- Space Complexity: \(O(m \times n)\), for the memoization table.

---

## Approach 3: Bottom-Up Dynamic Programming

**Intuition:**

The bottom-up approach fills a `dp` table, where each cell `dp[i][j]` represents the minimum distance to transform the first `i` characters of `word1` to the first `j` characters of `word2`. We can fill out this table using the base cases for transformations when either string is empty, and build upwards.

```csharp
public class Solution {
    public int MinDistance(string word1, string word2) {
        int m = word1.Length, n = word2.Length;
        int[,] dp = new int[m + 1, n + 1];
        
        // Base cases for transformations involving empty string
        for (int i = 0; i <= m; i++) dp[i, 0] = i;
        for (int j = 0; j <= n; j++) dp[0, j] = j;
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    // If characters are equal, no operation is needed
                    dp[i, j] = dp[i - 1, j - 1];
                } else {
                    // Consider all operations and choose the minimum
                    dp[i, j] = 1 + Math.Min(dp[i - 1, j - 1], // replace
                                              Math.Min(dp[i - 1, j], // delete
                                                       dp[i, j - 1])); // insert
                }
            }
        }
        
        return dp[m, n];
    }
}
```

**Complexity Analysis:**
- Time Complexity: \(O(m \times n)\).
- Space Complexity: \(O(m \times n)\), where `m` is `word1` length and `n` is `word2` length.

---

## Approach 4: Optimized Space Dynamic Programming

**Intuition:**

Since each state of `dp[i][j]` depends only on the previous and current row, we can further optimize the space usage by keeping only two rows at any time instead of the whole `dp` table.

```csharp
public class Solution {
    public int MinDistance(string word1, string word2) {
        int m = word1.Length, n = word2.Length;
        if (m < n) return MinDistance(word2, word1); // ensure m >= n for optimization
        
        int[] prev = new int[n + 1], curr = new int[n + 1];
        
        for (int j = 0; j <= n; j++) prev[j] = j;
        
        for (int i = 1; i <= m; i++) {
            curr[0] = i;
            for (int j = 1; j <= n; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    curr[j] = prev[j - 1];
                } else {
                    curr[j] = 1 + Math.Min(prev[j - 1], Math.Min(prev[j], curr[j - 1]));
                }
            }
            Array.Copy(curr, prev, n + 1); // updating reference for next iteration
        }
        
        return prev[n];
    }
}
```

**Complexity Analysis:**
- Time Complexity: \(O(m \times n)\).
- Space Complexity: \(O(n)\), as only two rows are stored.

This solution represents an ideal balance between time efficiency and minimal space consumption when calculating edit distances between two strings.

