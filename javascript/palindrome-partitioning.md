# [Leetcode 131: Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

## Table of Contents
- [Approach 1: Brute Force - Backtracking](#approach-1-brute-force---backtracking)
- [Approach 2: Recursive Backtracking with Dynamic Programming (Memoization)](#approach-2-recursive-backtracking-with-dynamic-programming-memoization)

### Approach 1: Brute Force - Backtracking

The problem asks us to partition a given string `s` such that every substring of the partition is a palindrome, and return all possible palindrome partitionings. The most intuitive approach is to use backtracking to explore all potential partitions of the string and check if each partition is a palindrome.

#### Intuition:
1. If the current substring is a palindrome, then make a cut and proceed with the rest of the string recursively.
2. Use backtracking to try every possible cut of the string.
3. Collect solutions that satisfy the palindrome condition for every partition.

#### Implementation:
```javascript
var partition = function(s) {
    const result = [];

    const isPalindrome = (str, left, right) => {
        while (left < right) {
            if (str[left] !== str[right]) return false;
            left++;
            right--;
        }
        return true;
    }

    const backtrack = (start, path) => {
        if (start === s.length) {
            result.push([...path]);
            return;
        }
        
        for (let end = start; end < s.length; end++) {
            if (isPalindrome(s, start, end)) {
                path.push(s.substring(start, end + 1));
                backtrack(end + 1, path);
                path.pop();
            }
        }
    }

    backtrack(0, []);
    return result;
};
```

#### Time Complexity:
- O(N * 2^N): There are 2^N possible partitions (each character can either be included in the current partition or start a new one), and checking if each partition is a palindrome takes O(N) time.
#### Space Complexity:
- O(N): Space required for path and stack in the worst case.

### Approach 2: Recursive Backtracking with Dynamic Programming (Memoization)

This approach builds upon the backtracking solution by using a dynamic programming table to store the results of previously computed palindrome checks, thus reducing repeated work.

#### Intuition:
1. Preprocess to fill a 2D table `dp` where `dp[i][j]` is `true` if the substring `s[i:j+1]` is a palindrome.
2. Use this `dp` table in the backtracking step to quickly verify if a substring is a palindrome.
3. As before, collect results of valid partitions.

#### Implementation:
```javascript
var partition = function(s) {
    const n = s.length;
    const result = [];
    
    // dynamic programming table to hold palindrome state
    const dp = Array.from({length: n}, () => Array(n).fill(false));
    
    // Fill the dp table: dp[i][j] is true if s[i:j+1] is a palindrome
    for (let right = 0; right < n; right++) {
        for (let left = 0; left <= right; left++) {
            if (s[left] === s[right] && (right - left <= 2 || dp[left + 1][right - 1])) {
                dp[left][right] = true;
            }
        }
    }
    
    const backtrack = (start, path) => {
        if (start === n) {
            result.push([...path]);
            return;
        }
        
        for (let end = start; end < n; end++) {
            if (dp[start][end]) { // Check palindrome status using dp
                path.push(s.substring(start, end + 1));
                backtrack(end + 1, path);
                path.pop();
            }
        }
    }
    
    backtrack(0, []);
    return result;
};
```

#### Time Complexity:
- O(N * 2^N): We still consider all partitions, but using the precomputed `dp` table speeds up palindrome checking to O(1).
#### Space Complexity:
- O(N^2): Additional space for the `dp` table storing palindrome status is needed, making the approach more memory-intensive than the basic backtracking method.

