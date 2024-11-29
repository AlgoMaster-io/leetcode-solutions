# 139. [Word Break](https://leetcode.com/problems/word-break/)

## Approach 1: Recursion with Memoization

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
package main

import "strings"

func wordBreak(s string, wordDict []string) bool {
    memo := make(map[string]bool)
    return wordBreakHelper(s, wordDict, memo)
}

func wordBreakHelper(s string, wordDict []string, memo map[string]bool) bool {
    if s == "" {
        return true
    }

    if val, exists := memo[s]; exists {
        return val
    }

    // Check if any prefix of 's' is a word in wordDict
    for _, word := range wordDict {
        if strings.HasPrefix(s, word) && wordBreakHelper(s[len(word):], wordDict, memo) {
            memo[s] = true
            return true
        }
    }

    memo[s] = false
    return false
}
```


## Approach 2: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
package main

func wordBreak(s string, wordDict []string) bool {
    wordSet := make(map[string]bool)
    for _, word := range wordDict {
        wordSet[word] = true
    }

    dp := make([]bool, len(s)+1)
    dp[0] = true // Base case, empty string

    // Traverse the string to build DP table
    for i := 1; i <= len(s); i++ {
        for j := 0; j < i; j++ {
            // If substring(0, j) can be segmented and substring(j, i) is in wordSet
            if dp[j] && wordSet[s[j:i]] {
                dp[i] = true
                break // Move on to next i since we found a valid segmentation
            }
        }
    }

    return dp[len(s)] // Result is whether the whole string can be segmented
}
```

Let me know if you need any further explanation!

