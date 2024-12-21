# [Leetcode 72: Edit Distance](https://leetcode.com/problems/edit-distance/)

## Approaches
- [Approach 1: Recursion (Top-Down) Approach](#approach-1-recursion-top-down-approach)
- [Approach 2: Recursion with Memoization (Top-Down Dynamic Programming)](#approach-2-recursion-with-memoization-top-down-dynamic-programming)
- [Approach 3: Dynamic Programming (Bottom-Up)](#approach-3-dynamic-programming-bottom-up)
- [Approach 4: Optimized Dynamic Programming with Reduced Space](#approach-4-optimized-dynamic-programming-with-reduced-space)

---

## Approach 1: Recursion (Top-Down) Approach
The Edit Distance problem is a classic dynamic programming problem which can be initially approached using a recursive method. The basic intuition here is to compare each character of the two strings and decide to insert, delete, or replace a character based on whether they match or not.

### Steps:
1. Compare the current characters of the two strings.
2. If they are equal, move to the next character of both strings.
3. If they are not equal, recursively find the minimum of the following operations:
   - Insert a character: Proceed with the next character in the second string.
   - Delete a character: Proceed with the next character in the first string.
   - Replace a character: Proceed with the next character for both strings.
4. Sum up the operations needed to transform the complete string.

### Code:
```javascript
function minDistance(word1, word2) {
    function helper(i, j) {
        // Base cases: If one string is empty, return the length of the other (all inserts/deletes).
        if (i === word1.length) return word2.length - j;
        if (j === word2.length) return word1.length - i;
        
        // If characters match, move to next pair
        if (word1[i] === word2[j]) {
            return helper(i + 1, j + 1);
        }
        
        // If not, consider all possibilities: insert, delete, replace
        const insertOp = helper(i, j + 1);
        const deleteOp = helper(i + 1, j);
        const replaceOp = helper(i + 1, j + 1);
        
        return 1 + Math.min(insertOp, deleteOp, replaceOp);
    }
    
    return helper(0, 0);
}
```

### Complexity:
- Time Complexity: O(3^(m+n)), where m and n are the lengths of the two strings. This is because for each mismatch we explore 3 options.
- Space Complexity: O(m+n), due to the recursion stack.

---

## Approach 2: Recursion with Memoization (Top-Down Dynamic Programming)
To improve on the pure recursive approach, we can use memoization to store previously computed results and avoid redundant calculations.

### Steps:
1. Use a memoization table to store results of subproblems.
2. Run the recursive logic but first check if the result for the current indexes is already computed.
3. Store the result in the memo table before returning.

### Code:
```javascript
function minDistance(word1, word2) {
    const memo = Array(word1.length).fill(null).map(() => Array(word2.length).fill(-1));
    
    function helper(i, j) {
        if (i === word1.length) return word2.length - j;
        if (j === word2.length) return word1.length - i;
        if (memo[i][j] !== -1) return memo[i][j];
        
        if (word1[i] === word2[j]) {
            return memo[i][j] = helper(i + 1, j + 1);
        }
        
        const insertOp = helper(i, j + 1);
        const deleteOp = helper(i + 1, j);
        const replaceOp = helper(i + 1, j + 1);
        
        return memo[i][j] = 1 + Math.min(insertOp, deleteOp, replaceOp);
    }
    
    return helper(0, 0);
}
```

### Complexity:
- Time Complexity: O(m*n), as each subproblem is solved only once.
- Space Complexity: O(m*n), for the memoization table and recursion stack.

---

## Approach 3: Dynamic Programming (Bottom-Up)
The bottom-up approach is essentially filling up a DP table iteratively based on the results of smaller subproblems. This generally helps in avoiding the overhead of a recursion stack.

### Steps:
1. Create a 2D DP table where dp[i][j] represents the edit distance between the first i characters of word1 and the first j characters of word2.
2. Initialize the first row and column of the table to represent the edit distances when one of the strings is empty.
3. Fill the table iteratively comparing each character and taking the minimum operation needed at each step.

### Code:
```javascript
function minDistance(word1, word2) {
    const m = word1.length, n = word2.length;
    const dp = Array(m + 1).fill(null).map(() => Array(n + 1).fill(0));
    
    // Base case: transforming empty string to another
    for (let i = 0; i <= m; i++) dp[i][0] = i;
    for (let j = 0; j <= n; j++) dp[0][j] = j;
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                // If characters are the same, no new operation needed
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                // Consider insert, delete, and replace operations
                const insertOp = dp[i][j - 1];
                const deleteOp = dp[i - 1][j];
                const replaceOp = dp[i - 1][j - 1];
                
                dp[i][j] = 1 + Math.min(insertOp, deleteOp, replaceOp);
            }
        }
    }
    
    return dp[m][n];
}
```

### Complexity:
- Time Complexity: O(m*n), filling up the DP table.
- Space Complexity: O(m*n), for the DP table.

---

## Approach 4: Optimized Dynamic Programming with Reduced Space
We can optimize the space complexity in the DP approach by realizing that to compute the current row in the DP table, we only need the current and the previous rows.

### Steps:
1. Use a single dimensional array to represent the current and previous state of the dynamic table.
2. Initialize the array for when one of the strings is empty.
3. Update the array iteratively using only the previous row's values.

### Code:
```javascript
function minDistance(word1, word2) {
    const m = word1.length, n = word2.length;
    let prev = Array(n + 1).fill(0);
    let curr = Array(n + 1).fill(0);
    
    for (let j = 0; j <= n; j++) prev[j] = j;
    
    for (let i = 1; i <= m; i++) {
        curr[0] = i;
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                curr[j] = prev[j - 1];
            } else {
                const insertOp = curr[j - 1];
                const deleteOp = prev[j];
                const replaceOp = prev[j - 1];
                
                curr[j] = 1 + Math.min(insertOp, deleteOp, replaceOp);
            }
        }
        [prev, curr] = [curr, prev];
    }
    
    return prev[n];
}
```

### Complexity:
- Time Complexity: O(m*n), filling up the array.
- Space Complexity: O(n), using a 1D array to store only the necessary previous and current computations.

In conclusion, these approaches showcase the evolution from a naive recursive method to an optimal space-efficient dynamic programming solution, effectively balancing time and space complexities.

