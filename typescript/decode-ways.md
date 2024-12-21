# [Leetcode 91: Decode Ways](https://leetcode.com/problems/decode-ways/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)

### Recursive Approach

#### Intuition:
The problem can be thought of as a decision tree where each node has two decisions:
- Decode a single character.
- Decode a pair of characters (if they form a valid number between 10 and 26).
  
Using recursion, we explore all possible decoding paths starting from the beginning of the string.

#### Code:
```typescript
function numDecodingsRecursive(s: string): number {
    function helper(index: number): number {
        // Base case: If index is at the end, we found a valid decoding
        if (index === s.length) return 1;
        
        // If the current character is '0', it can't be decoded
        if (s[index] === '0') return 0;
        
        // Decode single character
        let count = helper(index + 1);
        
        // Decode pair if valid (10 to 26)
        if (index + 1 < s.length && (s[index] === '1' || (s[index] === '2' && s[index + 1] <= '6'))) {
            count += helper(index + 2);
        }
        
        return count;
    }
    
    return helper(0);
}
```

#### Complexity:
- **Time Complexity**: O(2^n), where n is the length of the string. Each number can potentially create two branches (single and pair decoding).
- **Space Complexity**: O(n), for the recursive call stack.

---

### Memoization Approach

#### Intuition:
The recursive solution has overlapping subproblems. We can use memoization to store the result of subproblems to avoid redundant computations.

#### Code:
```typescript
function numDecodingsMemo(s: string): number {
    const memo: number[] = Array(s.length).fill(-1);
    
    function helper(index: number): number {
        // Base case: If index is at the end, we found a valid decoding
        if (index === s.length) return 1;
        
        // If the current character is '0', it can't be decoded
        if (s[index] === '0') return 0;
        
        // Check if result is already computed
        if (memo[index] !== -1) return memo[index];
        
        // Decode single character
        let count = helper(index + 1);
        
        // Decode pair if valid (10 to 26)
        if (index + 1 < s.length && (s[index] === '1' || (s[index] === '2' && s[index + 1] <= '6'))) {
            count += helper(index + 2);
        }
        
        // Store the result in memoization table
        memo[index] = count;
        
        return count;
    }
    
    return helper(0);
}
```

#### Complexity:
- **Time Complexity**: O(n), where n is the length of the string. Each subproblem is computed only once and stored.
- **Space Complexity**: O(n), for the memoization array and call stack.

---

### Dynamic Programming Approach

#### Intuition:
Instead of using a recursive approach, we'll use a DP table to iteratively build the solution. This avoids the recursion overhead and uses an iterative approach.

#### Code:
```typescript
function numDecodingsDP(s: string): number {
    if (s[0] === '0') return 0;
    
    const dp: number[] = Array(s.length + 1).fill(0);
    dp[0] = 1; // Base case: empty string
    dp[1] = 1; // Base case: non-zero character

    for (let i = 2; i <= s.length; i++) {
        // Single character decoding
        if (s[i - 1] !== '0') {
            dp[i] += dp[i - 1];
        }
        
        // Pair character decoding
        const twoDigit = parseInt(s.substring(i - 2, i));
        if (twoDigit >= 10 && twoDigit <= 26) {
            dp[i] += dp[i - 2];
        }
    }
    
    return dp[s.length];
}
```

#### Complexity:
- **Time Complexity**: O(n), where n is the length of the string. We iterate over the string once.
- **Space Complexity**: O(n), for the DP table storing solutions up to each index.

This problem highlights the strengths of recursive and dynamic programming techniques in solving complexity effectively.

