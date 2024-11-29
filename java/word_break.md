# 139. [Word Break](https://leetcode.com/problems/word-break/)

## Approach 1: Recursion with Memoization

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n)
import java.util.List;
import java.util.HashMap;

public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        return wordBreakHelper(s, wordDict, new HashMap<>());
    }

    private boolean wordBreakHelper(String s, List<String> wordDict, HashMap<String, Boolean> memo) {
        if (s.isEmpty()) {
            return true;
        }
        
        if (memo.containsKey(s)) {
            return memo.get(s);
        }

        // Check if any prefix of 's' is a word in wordDict
        for (String word : wordDict) {
            if (s.startsWith(word) && wordBreakHelper(s.substring(word.length()), wordDict, memo)) {
                memo.put(s, true);
                return true;
            }
        }

        memo.put(s, false);
        return false;
    }
}
```

## Approach 2: Dynamic Programming

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n)
import java.util.List;
import java.util.Set;
import java.util.HashSet;

public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordSet = new HashSet<>(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true; // Base case, empty string

        // Traverse the string to build DP table
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                // If substring(0, j) can be segmented and substring(j, i) is in wordSet
                if (dp[j] && wordSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break; // Move on to next i since we found a valid segmentation
                }
            }
        }

        return dp[s.length()]; // Result is whether the whole string can be segmented
    }
}
```

Let me know if you need any further explanation!

