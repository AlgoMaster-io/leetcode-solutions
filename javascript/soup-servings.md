## [Leetcode 808: Soup Servings](https://leetcode.com/problems/soup-servings/)

### Approaches
- [Basic Recursive Approach](#basic-recursive-approach)
- [Memoization](#memoization)
- [Dynamic Programming](#dynamic-programming)

### Basic Recursive Approach

#### Intuition:
The problem requires calculating the probability of serving all of soup A before soup B. The simplest solution is to use recursion, exploring each possible serving combination by decrementing soups A and B according to the given operations. We need to consider all possible ways to serve the soups and count the successful scenarios.

We have four possible serving actions:
1. (100, 0)
2. (75, 25)
3. (50, 50)
4. (25, 75)

Using these actions, we recursively calculate the probability of each action and base case will be when A or B is less than or equal to zero.

```javascript
function soupServings(N) {
    // Handling an edge case where serving size is large
    if (N > 5000) return 1;

    // Recursive helper function
    function helper(A, B) {
        // Base cases
        if (A <= 0 && B <= 0) return 0.5; // Both soups empty
        if (A <= 0) return 1; // A empty first
        if (B <= 0) return 0; // B empty first

        // Recursive case: evaluate all possible actions
        return 0.25 * (
            helper(A - 100, B) + 
            helper(A - 75, B - 25) + 
            helper(A - 50, B - 50) + 
            helper(A - 25, B - 75)
        );
    }
    
    // Initial call with soups equivalent to N/25 servings
    return helper(N, N);
}

console.log(soupServings(50)); // Sample Input
```

**Time Complexity**: Exponential (very high) due to overlapping subproblems.

**Space Complexity**: O(N) for call stack in case of depth N recursion.

### Memoization

#### Intuition:
Since recursive solutions involve recalculating many subproblems multiple times, we can use memoization to store results of subproblems. By storing the result of each soup serving state (A, B), we avoid redundant calculations, dramatically improving efficiency.

```javascript
function soupServings(N) {
    if (N > 5000) return 1;

    let memo = new Map();

    function helper(A, B) {
        if (A <= 0 && B <= 0) return 0.5;
        if (A <= 0) return 1;
        if (B <= 0) return 0;

        // Create a unique key for memoization
        const key = `${A},${B}`;
        if (memo.has(key)) return memo.get(key);
        
        let result = 0.25 * (
            helper(A - 100, B) + 
            helper(A - 75, B - 25) + 
            helper(A - 50, B - 50) + 
            helper(A - 25, B - 75)
        );
        
        memo.set(key, result);
        return result;
    }
    
    return helper(N, N);
}

console.log(soupServings(50)); // Sample Input
```

**Time Complexity**: O(N^2) due to exploring each combination once.

**Space Complexity**: O(N^2) for the memoization storage.

### Dynamic Programming

#### Intuition:
We can utilize dynamic programming by building a bottom-up solution. We create a DP table where each entry `dp[a][b]` represents the probability that soup A empties before soup B when there are `a` units of soup A and `b` units of soup B.

```javascript
function soupServings(N) {
    if (N > 5000) return 1;

    // Convert N to max appropriate serving units
    N = Math.ceil(N / 25);
    const dp = Array.from({length: N+1}, () => Array(N+1).fill(0));

    // Base cases
    for (let i = 1; i <= N; i++) {
        dp[0][i] = 1;  // If A is empty before B
    }
    dp[0][0] = 0.5;  // both A and B empty simulation

    // Fill DP table using bottom-up approach
    for (let a = 1; a <= N; a++) {
        for (let b = 1; b <= N; b++) {
            dp[a][b] = 0.25 * (
                dp[Math.max(0, a-4)][b] + 
                dp[Math.max(0, a-3)][Math.max(0, b-1)] +
                dp[Math.max(0, a-2)][Math.max(0, b-2)] +
                dp[Math.max(0, a-1)][Math.max(0, b-3)]
            );
        }
    }

    return dp[N][N];
}

console.log(soupServings(50)); // Sample Input
```

**Time Complexity**: O(N^2) as each state is computed only once.

**Space Complexity**: O(N^2) for the DP table.

Each approach improves on the last by optimizing calculation and storage of computed values. Dynamic programming offers the most efficient method by systematically calculating and storing values needed to determine the overall probability.

