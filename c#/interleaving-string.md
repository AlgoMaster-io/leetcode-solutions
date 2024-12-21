# [Leetcode 97: Interleaving String](https://leetcode.com/problems/interleaving-string/)

## Approaches:
- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

### Approach 1: Recursion
The most basic approach is to use recursion to determine if `s3` can be formed by interleaving `s1` and `s2`. At each step, we compare the current character of `s3` with characters of `s1` and `s2`. If they match, we recursively check the remaining strings.

#### Intuition:
1. If the current character of `s3` matches the current character of `s1`, we move to the next character in both `s1` and `s3`.
2. Similarly, we compare with `s2`.
3. We need to explore both possibilities wherever applicable (branching recursion).

```csharp
public class Solution {
    // Recursive function to check if s3 is an interleaving of s1 and s2
    private bool IsInterleaveRecursive(string s1, string s2, string s3, int i, int j, int k) {
        // Base case: if all strings are exhausted
        if (i == s1.Length && j == s2.Length && k == s3.Length) {
            return true;
        }
        
        // Check the current state of s3 with s1 and s2
        bool valid = false;
        
        // If current character in s1 matches current in s3, recurse further with s1
        if (i < s1.Length && s1[i] == s3[k]) {
            valid = IsInterleaveRecursive(s1, s2, s3, i + 1, j, k + 1);
        }
        
        // If current character in s2 matches current in s3, recurse further with s2
        if (!valid && j < s2.Length && s2[j] == s3[k]) {
            valid = IsInterleaveRecursive(s1, s2, s3, i, j + 1, k + 1);
        }        
        return valid;
    }
    
    public bool IsInterleave(string s1, string s2, string s3) {
        // Edge case: total length should match
        if (s1.Length + s2.Length != s3.Length) return false;
        return IsInterleaveRecursive(s1, s2, s3, 0, 0, 0);
    }
}
```
**Time Complexity:** Exponential O(2^(m+n)) where m and n are the lengths of `s1` and `s2` respectively. This is because each character addition has two possibilities.

**Space Complexity:** O(m + n) for the call stack.

### Approach 2: Recursion with Memoization
To optimize the recursive approach, we can store the results of already computed states using memoization. This avoids recalculating results for the same inputs.

#### Intuition:
- Memoization technique involves storing the results of recursive calls in a 2D array, where `dp[i][j]` indicates whether `s3[0...i+j]` is an interleaving of `s1[0...i]` and `s2[0...j]`.

```csharp
public class Solution {
    private bool[,] memo;
    
    private bool IsInterleaveMemo(string s1, string s2, string s3, int i, int j, int k) {
        if (i == s1.Length && j == s2.Length && k == s3.Length) {
            return true;
        }
        
        if (memo[i, j] != null) return memo[i, j];
        
        bool valid = false;
        
        if (i < s1.Length && s1[i] == s3[k]) {
            valid = IsInterleaveMemo(s1, s2, s3, i + 1, j, k + 1);
        }
        
        if (!valid && j < s2.Length && s2[j] == s3[k]) {
            valid = IsInterleaveMemo(s1, s2, s3, i, j + 1, k + 1);
        }
        
        memo[i, j] = valid;
        return valid;
    }
    
    public bool IsInterleave(string s1, string s2, string s3) {
        if (s1.Length + s2.Length != s3.Length) return false;
        
        memo = new bool?[s1.Length + 1, s2.Length + 1];
        return IsInterleaveMemo(s1, s2, s3, 0, 0, 0);
    }
}
```
**Time Complexity:** O(m*n) because each subproblem is computed once.

**Space Complexity:** O(m*n) for memoization table plus call stack space O(m+n).

### Approach 3: Dynamic Programming
This approach uses a bottom-up dynamic programming table to store results iteratively, avoiding the overhead of recursive calls.

#### Intuition:
- Create a 2D DP table `dp[i][j]` which signifies whether `s3[0...i+j]` can be formed using `s1[0...i]` and `s2[0...j]`.
- Initialize `dp[0][0]` as true, since two empty strings interleaving is another empty string.
- Fill the DP table based on the possibility of using characters from `s1` or `s2`.

```csharp
public class Solution {
    public bool IsInterleave(string s1, string s2, string s3) {
        if (s1.Length + s2.Length != s3.Length) return false;
        
        bool[,] dp = new bool[s1.Length + 1, s2.Length + 1];
        dp[0, 0] = true;
        
        for (int i = 0; i <= s1.Length; i++) {
            for (int j = 0; j <= s2.Length; j++) {
                if (i > 0 && s1[i - 1] == s3[i + j - 1]) {
                    dp[i, j] = dp[i, j] || dp[i - 1, j];
                }
                if (j > 0 && s2[j - 1] == s3[i + j - 1]) {
                    dp[i, j] = dp[i, j] || dp[i, j - 1];
                }
            }
        }
        
        return dp[s1.Length, s2.Length];
    }
}
```
**Time Complexity:** O(m*n) due to constructing a `dp` table.

**Space Complexity:** O(m*n) for the DP table.

By following these structured approaches, we can understand the problem deeply, optimize it step-by-step, and finally, implement a more efficient solution.

