# 139. [Word Break](https://leetcode.com/problems/word-break/)

## Approach 1: Recursion with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
function wordBreak(s: string, wordDict: string[]): boolean {
    return wordBreakHelper(s, wordDict, new Map<string, boolean>());
}

function wordBreakHelper(s: string, wordDict: string[], memo: Map<string, boolean>): boolean {
    if (s === "") {
        return true;
    }
    
    if (memo.has(s)) {
        return memo.get(s)!;
    }

    // Check if any prefix of 's' is a word in wordDict
    for (const word of wordDict) {
        if (s.startsWith(word) && wordBreakHelper(s.substring(word.length), wordDict, memo)) {
            memo.set(s, true);
            return true;
        }
    }

    memo.set(s, false);
    return false;
}
```

## Approach 2: Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n)
function wordBreak(s: string, wordDict: string[]): boolean {
    const wordSet = new Set<string>(wordDict);
    const dp = new Array<boolean>(s.length + 1).fill(false);
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

