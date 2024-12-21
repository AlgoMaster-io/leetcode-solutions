# [Leetcode 132: Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

## Approaches
- [Brute Force Recursive Approach](#brute-force-recursive-approach)
- [Memoization](#memoization)
- [Dynamic Programming](#dynamic-programming)

### Brute Force Recursive Approach

#### Intuition
We want to partition the string such that every substring is a palindrome, and we want to minimize the number of cuts. A naive approach is to use recursion to try every possible way of cutting the string. For every substring, check if it is a palindrome, then recursively try the remaining substring.

#### Approach
1. Define a helper function to check if a string is a palindrome.
2. Use a recursive function to iterate over the string and partition it if a valid palindrome is found.
3. For each valid partition, increment a cut count and recursively process the remaining part of the string.
4. Return the minimum of these cuts as the solution.

#### C# Code
```csharp
public class Solution {
    public int MinCut(string s) {
        return RecursivePartition(s, 0) - 1;
    }
    
    private int RecursivePartition(string s, int start) {
        if (start == s.Length) {
            return 0;
        }
        int minCuts = int.MaxValue;
        for (int end = start; end < s.Length; end++) {
            if (IsPalindrome(s, start, end)) {
                // 1 cut plus cuts needed for the rest
                minCuts = Math.Min(minCuts, 1 + RecursivePartition(s, end + 1));
            }
        }
        return minCuts;
    }
    
    private bool IsPalindrome(string s, int left, int right) {
        while (left < right) {
            if (s[left] != s[right]) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```

#### Time Complexity
- The time complexity is O(n * 2^n), where n is the length of the string. This is because for each of the n positions, we are making a decision to partition or not, leading to exponential possibilities.
  
#### Space Complexity
- The space complexity is O(n^2) due to the stack space for recursion and the substring operations.

### Memoization

#### Intuition
We notice that many subproblems are being recalculated, therefore, by storing the results of subproblems, we can significantly improve the performance.

#### Approach
1. Extend the recursive solution by using a dictionary to store and reuse previously computed results.
2. Only compute a result when it hasn't been computed yet.

#### C# Code
```csharp
public class Solution {
    public int MinCut(string s) {
        int[] memo = new int[s.Length];
        Array.Fill(memo, -1);
        return MemoizedPartition(s, 0, memo) - 1;
    }
    
    private int MemoizedPartition(string s, int start, int[] memo) {
        if (start == s.Length) {
            return 0;
        }
        
        if (memo[start] != -1) {
            return memo[start];
        }
        
        int minCuts = int.MaxValue;
        for (int end = start; end < s.Length; end++) {
            if (IsPalindrome(s, start, end)) {
                minCuts = Math.Min(minCuts, 1 + MemoizedPartition(s, end + 1, memo));
            }
        }
        
        memo[start] = minCuts;
        return minCuts;
    }
    
    private bool IsPalindrome(string s, int left, int right) {
        while (left < right) {
            if (s[left] != s[right]) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```

#### Time Complexity
- The time complexity is O(n^2), as each substring is checked once, and each result is stored in the memo array.

#### Space Complexity
- The space complexity is O(n^2), primarily to store the results of subproblems.

### Dynamic Programming

#### Intuition
To further optimize, we can use dynamic programming to fill out a table that keeps track of the minimum cuts needed for each substring. Construct a dp table where dp[i] represents the minimum cuts needed for substring (0 to i).

#### Approach
1. Use a 1-dimensional dp array to calculate the minimum cuts.
2. Use a boolean 2D pal array to store if a substring (i to j) is a palindrome.
3. Fill the dp table based on previously calculated pal and dp values.

#### C# Code
```csharp
public class Solution {
    public int MinCut(string s) {
        int n = s.Length;
        if (n == 0) return 0;
        
        bool[,] pal = new bool[n, n];
        int[] dp = new int[n];
        
        for (int i = 0; i < n; i++) {
            dp[i] = i; // maximum cuts = length - 1 cuts
            for (int j = 0; j <= i; j++) {
                if (s[j] == s[i] && (i - j < 2 || pal[j + 1, i - 1])) {
                    pal[j, i] = true;
                    // If start of string, no cuts needed, else 1 cut + dp for previous part
                    dp[i] = j == 0 ? 0 : Math.Min(dp[i], dp[j - 1] + 1);
                }
            }
        }
        
        return dp[n - 1];
    }
}
```

#### Time Complexity
- The time complexity is O(n^2) for filling out the dp and pal table.

#### Space Complexity
- The space complexity is O(n^2) for storing the results in the pal 2D array and O(n) for dp array.

