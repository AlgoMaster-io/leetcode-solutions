# [Leetcode 1143: Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

## Approaches
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Recursive Solution with Memoization](#approach-2-recursive-solution-with-memoization)
- [Approach 3: Dynamic Programming (Tabulation)](#approach-3-dynamic-programming-tabulation)
- [Approach 4: Dynamic Programming with Space Optimization](#approach-4-dynamic-programming-with-space-optimization)

---

## Approach 1: Recursive Solution

### Intuition
The Longest Common Subsequence (LCS) problem can be solved using a simple recursive solution. The key idea is to explore all possible substrings of the two given strings and compare their characters. If characters match, we include them in our subsequence, and if they don't match, we explore further possibilities by excluding one character at a time.

### Steps
1. Base Case: If either string is empty, the LCS is 0.
2. Recursive Case: Compare characters at the current indices.
   - If they match, add 1 to the result and recurse for the remaining parts of both strings.
   - If they don't match, find the maximum LCS by:
     - Excluding the current character of the first string.
     - Excluding the current character of the second string.

### Code

```javascript
function longestCommonSubsequence(text1, text2) {
    function lcsRecursive(index1, index2) {
        // Base case: If either string is empty
        if (index1 >= text1.length || index2 >= text2.length) {
            return 0;
        }
        
        // If characters match, add 1 to the result and recurse for remaining strings
        if (text1[index1] === text2[index2]) {
            return 1 + lcsRecursive(index1 + 1, index2 + 1);
        } else {
            // Else, find the maximum LCS by excluding one character at a time
            return Math.max(lcsRecursive(index1 + 1, index2), lcsRecursive(index1, index2 + 1));
        }
    }
    
    return lcsRecursive(0, 0);
}
```

### Complexity
- Time Complexity: O(2^(m+n)) where m and n are the lengths of text1 and text2. This is due to the exponential number of recursive calls.
- Space Complexity: O(m+n) due to the recursion stack.

---

## Approach 2: Recursive Solution with Memoization

### Intuition
We can optimize the recursive solution by caching the results of subproblems using memoization. This avoids the recomputation of results for states already solved.

### Steps
1. Use a 2D array `memo` where `memo[i][j]` stores the LCS for the substrings starting at `i` in `text1` and `j` in `text2`.
2. When encountering a calculated state, return the cached value.

### Code

```javascript
function longestCommonSubsequence(text1, text2) {
    const memo = Array(text1.length).fill(null).map(() => Array(text2.length).fill(-1));

    function lcsMemo(index1, index2) {
        // Base case
        if (index1 >= text1.length || index2 >= text2.length) {
            return 0;
        }
        
        // Return the cached value if present
        if (memo[index1][index2] !== -1) {
            return memo[index1][index2];
        }

        if (text1[index1] === text2[index2]) {
            // Store calculated value in memo
            memo[index1][index2] = 1 + lcsMemo(index1 + 1, index2 + 1);
        } else {
            memo[index1][index2] = Math.max(lcsMemo(index1 + 1, index2), lcsMemo(index1, index2 + 1));
        }
        
        return memo[index1][index2];
    }
    
    return lcsMemo(0, 0);
}
```

### Complexity
- Time Complexity: O(m*n) as we solve each subproblem once.
- Space Complexity: O(m*n) for the memo table and recursion stack.

---

## Approach 3: Dynamic Programming (Tabulation)

### Intuition
Using dynamic programming, we can fill a table in a bottom-up manner. This avoids recursion and uses an iterative approach to build the solution.

### Steps
1. Create a 2D dp array where `dp[i][j]` represents the LCS of substrings `text1[0...i-1]` and `text2[0...j-1]`.
2. Initialize `dp[0][j] = 0` and `dp[i][0] = 0` because LCS with an empty string is 0.
3. Fill in the table iteratively comparing characters and store the best subproblem solution.

### Code

```javascript
function longestCommonSubsequence(text1, text2) {
    const dp = Array(text1.length + 1).fill(null).map(() => Array(text2.length + 1).fill(0));
    
    for (let i = 1; i <= text1.length; i++) {
        for (let j = 1; j <= text2.length; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }

    return dp[text1.length][text2.length];
}
```

### Complexity
- Time Complexity: O(m*n) because we iterate through each element of the dp table.
- Space Complexity: O(m*n) for the dp table.

---

## Approach 4: Dynamic Programming with Space Optimization

### Intuition
We can optimize the space usage of the table by realizing that to fill the current row in `dp`, we only need the current and previous row. Thus, we can keep just two arrays instead of the entire table.

### Steps
1. Use two 1D arrays: `previous` and `current`.
2. Iterate through each character pair, updating `current` while referring to `previous`.
3. Swap the arrays for the next row computation.

### Code

```javascript
function longestCommonSubsequence(text1, text2) {
    let previous = Array(text2.length + 1).fill(0);
    let current = Array(text2.length + 1).fill(0);

    for (let i = 1; i <= text1.length; i++) {
        for (let j = 1; j <= text2.length; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                current[j] = previous[j - 1] + 1;
            } else {
                current[j] = Math.max(previous[j], current[j - 1]);
            }
        }
        [previous, current] = [current, previous]; // Swap arrays
    }
    
    return previous[text2.length];
}
```

### Complexity
- Time Complexity: O(m*n) due to iterating through the matrix.
- Space Complexity: O(n) as we only store two rows at a time.

---

By following these approaches, you can effectively solve the LCS problem in a variety of ways, choosing the one that best suits your needs in terms of simplicity or optimization.

