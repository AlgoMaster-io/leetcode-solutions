# [Edit Distance - Leetcode 72](https://leetcode.com/problems/edit-distance/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)
4. [Dynamic Programming with Space Optimization](#dynamic-programming-with-space-optimization)

### Recursive Approach

This is the most basic solution using recursion. The idea is to break down the problem into subproblems by comparing characters of the two strings:

- If the characters are the same, move to the next pair of characters.
- If different, consider all possibilities:
  - Insert a character.
  - Remove a character.
  - Replace a character.
- The minimum of these possibilities gives the answer.

```typescript
function minDistance(word1: string, word2: string): number {
    function helper(i: number, j: number): number {
        // If first string is empty, we need to insert all characters of the second string
        if (i === 0) return j;
        // If second string is empty, we need to remove all characters of the first string
        if (j === 0) return i;

        // If characters are the same, move to the next pair
        if (word1[i - 1] === word2[j - 1]) {
            return helper(i - 1, j - 1);
        }

        // If they are different, consider all possibilities
        return 1 + Math.min(
            helper(i, j - 1),   // Insert
            helper(i - 1, j),   // Remove
            helper(i - 1, j - 1) // Replace
        );
    }
    return helper(word1.length, word2.length);
}
```

**Time Complexity**: O(3^(m+n)), where `m` and `n` are the lengths of `word1` and `word2`.  
**Space Complexity**: O(m+n) for the recursion stack.

### Memoization Approach

To optimize the recursive solution, use memoization to store results of subproblems and avoid redundant calculations.

```typescript
function minDistance(word1: string, word2: string): number {
    const memo: number[][] = Array(word1.length + 1).fill(null).map(() => Array(word2.length + 1).fill(-1));
    
    function helper(i: number, j: number): number {
        if (i === 0) return j;
        if (j === 0) return i;
        if (memo[i][j] !== -1) return memo[i][j];

        if (word1[i - 1] === word2[j - 1]) {
            memo[i][j] = helper(i - 1, j - 1);
        } else {
            memo[i][j] = 1 + Math.min(
                helper(i, j - 1),
                helper(i - 1, j),
                helper(i - 1, j - 1)
            );
        }
        return memo[i][j];
    }
    return helper(word1.length, word2.length);
}
```

**Time Complexity**: O(m*n)  
**Space Complexity**: O(m*n) due to memoization table and recursion stack.

### Dynamic Programming Approach

This approach uses a 2D DP table to iteratively build up the solution. It avoids recursion altogether:

```typescript
function minDistance(word1: string, word2: string): number {
    const dp: number[][] = Array(word1.length + 1).fill(null).map(() => Array(word2.length + 1).fill(0));

    // Fill the base cases
    for (let i = 0; i <= word1.length; i++) {
        dp[i][0] = i;  // All deletes
    }
    for (let j = 0; j <= word2.length; j++) {
        dp[0][j] = j;  // All inserts
    }

    // Fill the dp table
    for (let i = 1; i <= word1.length; i++) {
        for (let j = 1; j <= word2.length; j++) {
            if (word1[i-1] === word2[j-1]) {
                dp[i][j] = dp[i-1][j-1];
            } else {
                dp[i][j] = 1 + Math.min(
                    dp[i][j-1],  // Insert
                    dp[i-1][j],  // Remove
                    dp[i-1][j-1] // Replace
                );
            }
        }
    }
    return dp[word1.length][word2.length];
}
```

**Time Complexity**: O(m*n)  
**Space Complexity**: O(m*n)

### Dynamic Programming with Space Optimization

We can reduce the space complexity by using only two arrays because we only need the previous row to compute the current row.

```typescript
function minDistance(word1: string, word2: string): number {
    const previous: number[] = Array(word2.length + 1).fill(0);
    const current: number[] = Array(word2.length + 1).fill(0);

    // Fill the base case for converting empty string to word2
    for (let j = 0; j <= word2.length; j++) {
        previous[j] = j;
    }

    for (let i = 1; i <= word1.length; i++) {
        current[0] = i;  // Fill the base case for converting word1 to empty string

        for (let j = 1; j <= word2.length; j++) {
            if (word1[i-1] === word2[j-1]) {
                current[j] = previous[j-1];
            } else {
                current[j] = 1 + Math.min(
                    current[j-1],  // Insert
                    previous[j],   // Remove
                    previous[j-1]  // Replace
                );
            }
        }
        // Swap current and previous
        [previous, current] = [current, previous];
    }
    return previous[word2.length];
}
```

**Time Complexity**: O(m*n)  
**Space Complexity**: O(n) as only two arrays are used instead of a full 2D matrix.


