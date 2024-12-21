## [Distinct Subsequences - LeetCode](https://leetcode.com/problems/distinct-subsequences/)

### Table of Contents
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Recursive Approach with Memoization](#approach-2-recursive-approach-with-memoization)
- [Approach 3: Dynamic Programming - Bottom-Up](#approach-3-dynamic-programming-bottom-up)

### Approach 1: Recursive Approach
The recursive approach is the most straightforward way to solve this problem. We aim to determine the number of distinct subsequences of `s` that equal `t`. 

#### Intuition:
1. If `t` is empty, there is exactly one subsequence of `s` which equals `t`: the empty subsequence.
2. If `s` is empty but `t` is not, there are zero subsequences of `s` that equals `t`.
3. If the last characters of `s` and `t` match, we have a choice:
   - Include this character in our matching computation.
   - Skip this character.
4. If the last characters do not match, we must skip the character in `s`.

The recursive relation can be described as:
```
numDistinct(s, t) = numDistinct(s-1, t) + (lastCharsMatch ? numDistinct(s-1, t-1) : 0)
```

#### Code:
```csharp
public class Solution 
{
    public int NumDistinct(string s, string t) 
    {
        return NumDistinctRecursive(s, t, s.Length, t.Length);
    }
    
    private int NumDistinctRecursive(string s, string t, int m, int n)
    {
        // If t is empty, there's one subsequence: the empty subsequence
        if (n == 0) return 1;
        
        // If s is empty and t is not, there are no subsequences
        if (m == 0) return 0;
        
        // If last characters match
        if (s[m - 1] == t[n - 1])
        {
            // Include or exclude the last character of s
            return NumDistinctRecursive(s, t, m - 1, n - 1) + NumDistinctRecursive(s, t, m - 1, n);
        }
        else
        {
            // Exclude the last character of s
            return NumDistinctRecursive(s, t, m - 1, n);
        }
    }
}
```

#### Complexity:
- **Time Complexity:** \(O(2^m)\), where \(m\) is the length of `s`.
- **Space Complexity:** \(O(m + n)\) due to the recursion stack.

### Approach 2: Recursive Approach with Memoization
We can optimize the recursive method by storing the results of previously solved subproblems.

#### Intuition:
- Use a 2D table to store results of subproblems to avoid recomputation.

#### Code:
```csharp
public class Solution 
{
    public int NumDistinct(string s, string t) 
    {
        int[,] memo = new int[s.Length + 1, t.Length + 1];
        for(int i = 0; i < memo.GetLength(0); i++)
            for(int j = 0; j < memo.GetLength(1); j++)
                memo[i, j] = -1;
        
        return NumDistinctMemo(s, t, s.Length, t.Length, memo);
    }
    
    private int NumDistinctMemo(string s, string t, int m, int n, int[,] memo)
    {
        if (n == 0) return 1;
        if (m == 0) return 0;

        if (memo[m, n] != -1) return memo[m, n];
        
        if (s[m - 1] == t[n - 1])
        {
            memo[m, n] = NumDistinctMemo(s, t, m - 1, n - 1, memo) 
                         + NumDistinctMemo(s, t, m - 1, n, memo);
        }
        else
        {
            memo[m, n] = NumDistinctMemo(s, t, m - 1, n, memo);
        }
        
        return memo[m, n];
    }
}
```

#### Complexity:
- **Time Complexity:** \(O(m \cdot n)\), where \(m\) and \(n\) are the lengths of `s` and `t` respectively.
- **Space Complexity:** \(O(m \cdot n)\).

### Approach 3: Dynamic Programming - Bottom-Up

This approach uses an iterative solution with a DP table to store the number of distinct subsequences for substrings of `s` and `t`.

#### Intuition:
- Construct a 2D DP table `dp` where `dp[i][j]` represents the number of distinct subsequences of `s[0...i-1]` that equals `t[0...j-1]`.
- Fill this table using the relations derived from the recursive approach.

#### Code:
```csharp
public class Solution 
{
    public int NumDistinct(string s, string t) 
    {
        int m = s.Length, n = t.Length;
        int[,] dp = new int[m + 1, n + 1];

        for (int i = 0; i <= m; i++)
        {
            dp[i, 0] = 1; // Empty string t can be formed by any prefix of s
        }

        for (int i = 1; i <= m; i++)
        {
            for (int j = 1; j <= n; j++)
            {
                if (s[i - 1] == t[j - 1])
                {
                    dp[i, j] = dp[i - 1, j - 1] + dp[i - 1, j];
                }
                else
                {
                    dp[i, j] = dp[i - 1, j];
                }
            }
        }

        return dp[m, n];
    }
}
```

#### Complexity:
- **Time Complexity:** \(O(m \cdot n)\), where \(m\) and \(n\) are the lengths of `s` and `t`.
- **Space Complexity:** \(O(m \cdot n)\). We can further optimize this to \(O(n)\) with a single-dimensional temporary array to save space.

