# [Leetcode Problem 97: Interleaving String](https://leetcode.com/problems/interleaving-string/)

## Approaches
- [Approach 1: Recursion with Memoization](#approach-1-recursion-with-memoization)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

### Approach 1: Recursion with Memoization

**Intuition**:  
- The problem involves deciding if a third string s3 can be formed by interleaving two other strings s1 and s2. This indicates a decision-making problem where, at each step, you decide whether to take the current character from s1 or s2.
- A recursive solution can help us by choosing characters from either s1 or s2. However, recursion alone will result in recomputation of states, which we can optimize using memoization.

**Steps**:
1. Define a recursive function `isInterleaveRecurse` that takes the indices `i`, `j` of strings `s1` and `s2` respectively, and checks if by selecting characters from these indices onward, s3 can be formed.
2. Base case: If both indices have traversed the entire length of `s1` and `s2`, verify if `s3` is also completely traversed.
3. Check if the current character of `s1` or `s2` matches with the current character of `s3`.
    - If a match is found, make a recursive call after moving the respective index forward.
4. Use a memoization table to store already computed results for specific `i`, `j` indices.

```typescript
function isInterleave(s1: string, s2: string, s3: string): boolean {
    if (s1.length + s2.length !== s3.length) return false;

    const memo: Map<string, boolean> = new Map();

    function isInterleaveRecurse(i: number, j: number, k: number): boolean {
        // Base case when all strings are completely checked
        if (i === s1.length && j === s2.length && k === s3.length) return true;
        
        const memoKey = `${i}-${j}-${k}`;
        if (memo.has(memoKey)) return memo.get(memoKey)!;

        if (i < s1.length && s1[i] === s3[k] && isInterleaveRecurse(i+1, j, k+1)) {
            memo.set(memoKey, true);
            return true;
        }

        if (j < s2.length && s2[j] === s3[k] && isInterleaveRecurse(i, j+1, k+1)) {
            memo.set(memoKey, true);
            return true;
        }
        
        memo.set(memoKey, false);
        return false;
    }

    return isInterleaveRecurse(0, 0, 0);
}

```

- **Time Complexity**: O(n * m), where n and m are the lengths of `s1` and `s2`. Each state of (i, j) is computed once.
- **Space Complexity**: O(n * m) for the memoization table.

### Approach 2: Dynamic Programming

**Intuition**:  
- We'll use a bottom-up dynamic programming approach to store results of subproblems and build up to the solution for the whole problem.
- Define `dp[i][j]` to be true if `s3[0:i+j - 1]` can be formed by interleaving `s1[0:i - 1]` and `s2[0:j - 1]`.

**Steps**:
1. Initialize a 2D boolean array `dp` where `dp[i][j]` indicates whether `s3[0:i+j-1]` is an interleaving of `s1[0:i-1]` and `s2[0:j-1]`.
2. Set `dp[0][0]` to true since an empty `s1` and `s2` can make an empty `s3`.
3. Fill `dp[i][j]` considering if the character from `s1` or `s2` matches with `s3` and if the previous state was true.

```typescript
function isInterleave(s1: string, s2: string, s3: string): boolean {
    const n = s1.length, m = s2.length;
    if (n + m !== s3.length) return false;

    const dp: boolean[][] = Array(n + 1).fill(0).map(() => Array(m + 1).fill(false));
    dp[0][0] = true; // base case

    for (let i = 0; i <= n; i++) {
        for (let j = 0; j <= m; j++) {
            const k = i + j - 1; // current index in s3
            if (i > 0) {
                // if the previous state was true and current character of s1 matches s3
                dp[i][j] = dp[i][j] || (dp[i - 1][j] && s1[i - 1] === s3[k]);
            }
            if (j > 0) {
                // if the previous state was true and current character of s2 matches s3
                dp[i][j] = dp[i][j] || (dp[i][j - 1] && s2[j - 1] === s3[k]);
            }
        }
    }

    return dp[n][m];
}

```

- **Time Complexity**: O(n * m), due to the double loop over `s1` and `s2`.
- **Space Complexity**: O(n * m), for storing results in the DP table.

