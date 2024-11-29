# 139. [Word Break](https://leetcode.com/problems/word-break/)

## Approach 1: Recursion with Memoization

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public bool WordBreak(string s, IList<string> wordDict) {
        return WordBreakHelper(s, new HashSet<string>(wordDict), new Dictionary<string, bool>());
    }

    private bool WordBreakHelper(string s, HashSet<string> wordDict, Dictionary<string, bool> memo) {
        if (string.IsNullOrEmpty(s)) {
            return true;
        }
        
        if (memo.ContainsKey(s)) {
            return memo[s];
        }

        // Check if any prefix of 's' is a word in wordDict
        foreach (string word in wordDict) {
            if (s.StartsWith(word) && WordBreakHelper(s.Substring(word.Length), wordDict, memo)) {
                memo[s] = true;
                return true;
            }
        }

        memo[s] = false;
        return false;
    }
}
```

## Approach 2: Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public bool WordBreak(string s, IList<string> wordDict) {
        HashSet<string> wordSet = new HashSet<string>(wordDict);
        bool[] dp = new bool[s.Length + 1];
        dp[0] = true; // Base case, empty string

        // Traverse the string to build DP table
        for (int i = 1; i <= s.Length; i++) {
            for (int j = 0; j < i; j++) {
                // If substring(0, j) can be segmented and substring(j, i) is in wordSet
                if (dp[j] && wordSet.Contains(s.Substring(j, i - j))) {
                    dp[i] = true;
                    break; // Move on to next i since we found a valid segmentation
                }
            }
        }

        return dp[s.Length]; // Result is whether the whole string can be segmented
    }
}
```

