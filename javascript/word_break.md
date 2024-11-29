# 139. [Word Break](https://leetcode.com/problems/word-break/)

## Approach 1: Recursion with Memoization

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
function wordBreak(s, wordDict) {
    return wordBreakHelper(s, wordDict, {});
}

function wordBreakHelper(s, wordDict, memo) {
    if (s === "") {
        return true;
    }
    
    if (s in memo) {
        return memo[s];
    }

    // Check if any prefix of 's' is a word in wordDict
    for (let word of wordDict) {
        if (s.startsWith(word) && wordBreakHelper(s.substring(word.length), wordDict, memo)) {
            memo[s] = true;
            return true;
        }
    }

    memo[s] = false;
    return false;
}
```

## Approach 2: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
function wordBreak(s, wordDict) {
    const wordSet = new Set(wordDict);
    const dp = Array(s.length + 1).fill(false);
    dp[0] = true; // Base case, empty string

    // Traverse the string to build DP table
    for (let i = 1; i <= s.length; i++) {
        for (let j = 0; j < i; j++) {
            // If substring(0, j) can be segmented and substring(j, i) is in wordSet
            if (dp[j] && wordSet.has(s.substring(j, i))) {
                dp[i] = true;
                break; // Move on to next i since we found a valid segmentation
            }
        }
    }

    return dp[s.length]; // Result is whether the whole string can be segmented
}
```

