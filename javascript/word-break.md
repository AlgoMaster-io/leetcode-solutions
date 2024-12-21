# [Leetcode 139: Word Break](https://leetcode.com/problems/word-break/)

## Approaches
- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Recursion + Memoization (Top-Down DP)](#approach-2-recursion--memoization-top-down-dp)
- [Approach 3: Dynamic Programming (Bottom-Up DP)](#approach-3-dynamic-programming-bottom-up-dp)

---

## Approach 1: Recursion

### Intuition:
We can solve this problem using a simple recursive approach. The idea is to keep checking all possible segments of the string starting from the first character and check if the segment is a word from the dictionary.

### Steps:
1. If the entire `s` is empty, it means we've successfully segmented it, so return `true`.
2. Iterate over all the prefixes of `s`.
3. If any prefix is found in the dictionary (`wordDict`), recursively check if the remaining string can be broken down.
4. If all prefixes are checked and no solution is found, return `false`.

### Code:

```javascript
var wordBreak = function(s, wordDict) {
    const wordSet = new Set(wordDict);
    
    function canBreak(start) {
        // base case: if start reached end, return true
        if (start === s.length) return true;
        
        // iterate through the string from the starting index
        for (let end = start + 1; end <= s.length; end++) {
            // check if the current prefix is in the word set
            if (wordSet.has(s.substring(start, end))) {
                // recursively check remaining string
                if (canBreak(end)) {
                    return true;
                }
            }
        }
        return false;
    }
    
    return canBreak(0);
};
```

### Complexity:
- **Time complexity**: O(2^n) in the worst case, where n is the length of `s`.
- **Space complexity**: O(n) due to recursion stack space.

---

## Approach 2: Recursion + Memoization (Top-Down DP)

### Intuition:
We can improve the recursive approach by using memoization to store results of subproblems. This avoids recomputing results for the same subproblems, thus reducing redundant calculations.

### Steps:
1. Use a `memo` array where `memo[i]` is `true` if the substring `s[i:]` can be segmented.
2. Initialize the memoization array to save results for previously computed indices.
3. Follow a similar recursive structure but save and reuse results.

### Code:

```javascript
var wordBreak = function(s, wordDict) {
    const wordSet = new Set(wordDict);
    const memo = Array(s.length).fill(undefined);
    
    function canBreak(start) {
        if (start === s.length) return true;
        if (memo[start] !== undefined) return memo[start];

        for (let end = start + 1; end <= s.length; end++) {
            if (wordSet.has(s.substring(start, end))) {
                if (canBreak(end)) {
                    memo[start] = true;
                    return true;
                }
            }
        }
        memo[start] = false;
        return false;
    }
    
    return canBreak(0);
};
```

### Complexity:
- **Time complexity**: O(n^2) with memoization because each substring is evaluated once.
- **Space complexity**: O(n) for memoization array and recursion stack.

---

## Approach 3: Dynamic Programming (Bottom-Up DP)

### Intuition:
We can conduct a bottom-up dynamic programming approach, which systematically builds up the solution by considering longer substrings starting from the shortest. This avoids the overhead of recursion.

### Steps:
1. Create a DP array `dp` where `dp[i]` will be `true` if the substring `s[0:i]` can be segmented.
2. Initialize `dp[0] = true` assuming the empty string can be segmented.
3. For every position `i`, check all possibilities of breaking it with combinations of smaller substrings that are also in the dictionary.
4. If `dp[n]` is `true`, the entire string can be segmented.

### Code:

```javascript
var wordBreak = function(s, wordDict) {
    const wordSet = new Set(wordDict);
    const dp = Array(s.length + 1).fill(false);
    dp[0] = true;

    for (let i = 1; i <= s.length; i++) {
        for (let j = 0; j < i; j++) {
            if (dp[j] && wordSet.has(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[s.length];
};
```

### Complexity:
- **Time complexity**: O(n^2), where n is the size of `s`.
- **Space complexity**: O(n), space for the DP array.

