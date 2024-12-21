# [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)
4. [Optimized Dynamic Programming Approach](#optimized-dynamic-programming-approach)

---

### Recursive Approach

The problem can be visualized as a decision tree where at each step, you can either move one step or two steps forward. The recursive approach explores all possible paths to reach the top.

The intuition is that the number of ways to reach the nth step is the sum of ways to reach the (n-1)th and the (n-2)th steps.

#### Code

```javascript
var climbStairs = function(n) {
    if (n <= 2) {
        return n;
    }
    return climbStairs(n - 1) + climbStairs(n - 2);
};

// Example Usage
console.log(climbStairs(5)); // Output: 8
```

#### Complexity Analysis
- **Time Complexity**: O(2^n) - There are 2^n possible calls.
- **Space Complexity**: O(n) - The depth of the recursion tree can go up to n.

---

### Memoization Approach

To reduce redundant calculations in the recursive approach, we can use memoization. We store the result of subproblems so that each subproblem is solved only once.

#### Code

```javascript
var climbStairs = function(n) {
    // Initialize a memoization array
    let memo = new Array(n + 1).fill(0);
    
    // Helper function to use memoization
    function climb(n) {
        if (n <= 2) {
            return n;
        }
        if (memo[n] > 0) {
            return memo[n]; // Return cached result
        }
        // Store result in memo before returning
        memo[n] = climb(n - 1) + climb(n - 2);
        return memo[n];
    }
  
    return climb(n);
};

// Example Usage
console.log(climbStairs(5)); // Output: 8
```

#### Complexity Analysis
- **Time Complexity**: O(n) - Each number from 1 to n is computed only once.
- **Space Complexity**: O(n) - Memoization array.

---

### Dynamic Programming Approach

Instead of recursion, we can use a dynamic programming approach to build the solution iteratively. This prevents stack overflow issues and improves readability.

The idea is to store the number of ways to reach each step in an array.

#### Code

```javascript
var climbStairs = function(n) {
    if (n <= 2) {
        return n;
    }
    
    // Initialize the base cases
    let dp = new Array(n + 1).fill(0);
    dp[1] = 1;
    dp[2] = 2;
    
    // Build the dp table iteratively
    for (let i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
};

// Example Usage
console.log(climbStairs(5)); // Output: 8
```

#### Complexity Analysis
- **Time Complexity**: O(n) - Single pass through the steps.
- **Space Complexity**: O(n) - Storage array of size n+1.

---

### Optimized Dynamic Programming Approach

This approach optimizes the dynamic programming solution further by using only two variables instead of a complete array to store previous results. This is possible because to calculate the number of ways to the current step, we only need the last two results.

#### Code

```javascript
var climbStairs = function(n) {
    if (n <= 2) {
        return n;
    }
    
    let first = 1;
    let second = 2;
    
    // Iterate from the 3rd step to the nth step
    for (let i = 3; i <= n; i++) {
        let current = first + second; // Calculate ways to i-th step
        first = second; // Shift one step up
        second = current; // Shift one step up
    }
    return second; // second holds the result for the n-th step
};

// Example Usage
console.log(climbStairs(5)); // Output: 8
```

#### Complexity Analysis
- **Time Complexity**: O(n) - Single loop from 3 to n.
- **Space Complexity**: O(1) - Constant space used for `first` and `second` variables.

