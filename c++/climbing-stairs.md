# [Climbing Stairs - Leetcode Problem 70](https://leetcode.com/problems/climbing-stairs/)

## Approaches
- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Recursion with Memorization](#approach-2-recursion-with-memorization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)
- [Approach 4: Fibonacci Formula Approach](#approach-4-fibonacci-formula-approach)

## Approach 1: Recursion

### Intuition
The problem of climbing stairs can be thought of as a variation of the Fibonacci sequence. At each step, we have the choice to either advance by one step or two steps. This forms a recursive structure where to reach step `n`, we must have come from either step `n-1` or step `n-2`.

### Code
```cpp
int climbStairs(int n) {
    // Base case: If there are 0 or 1 steps, there's only one way to climb them.
    if (n <= 1) return 1;
    // Recursive call: the number of ways to reach step `n` is the sum of
    // the number of ways to reach step `n - 1` and step `n - 2`.
    return climbStairs(n - 1) + climbStairs(n - 2);
}
```

### Complexity
- **Time Complexity:** O(2^n) - This solution is highly inefficient as it calculates the same Fibonacci value multiple times.
- **Space Complexity:** O(n) - The maximum depth of the recursion stack.

## Approach 2: Recursion with Memorization

### Intuition
To optimize the recursive solution, we can store previously calculated results in a memo array (or hashmap) to avoid redundant calculations. This technique is known as "memorization."

### Code
```cpp
int climbStairsMemo(int n, std::unordered_map<int, int>& memo) {
    // Check if the result for `n` is already computed.
    if (memo.find(n) != memo.end()) return memo[n];
    // Base case for steps 0 or 1
    if (n <= 1) return 1;
    // Store the result of the relation in memo and return it.
    memo[n] = climbStairsMemo(n - 1, memo) + climbStairsMemo(n - 2, memo);
    return memo[n];
}

int climbStairs(int n) {
    // Hash map to store computed results
    std::unordered_map<int, int> memo;
    return climbStairsMemo(n, memo);
}
```

### Complexity
- **Time Complexity:** O(n) - Each Fibonacci number up to `n` is computed only once.
- **Space Complexity:** O(n) - The call stack and hashmap store results for each step up to `n`.

## Approach 3: Dynamic Programming

### Intuition
The dynamic programming approach iteratively builds the solutions from the simplest subproblem to the problem of interest, thereby avoiding the overhead of function calls and recursion stack used in previous approaches.

### Code
```cpp
int climbStairs(int n) {
    // Base cases
    if (n <= 1) return 1;
    
    // Initialize the dp array where dp[i] means the number of ways to reach step i
    std::vector<int> dp(n + 1);
    dp[0] = 1; // one way to be on the ground (do nothing)
    dp[1] = 1; // one way to reach the first step (take one step)
    
    for (int i = 2; i <= n; ++i) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}
```

### Complexity
- **Time Complexity:** O(n) - We compute the result iteratively using a single pass.
- **Space Complexity:** O(n) - Storing the number of ways to each step in the `dp` array.

## Approach 4: Fibonacci Formula Approach

### Intuition
Since the problem is a variation of the Fibonacci sequence, we can leverage the closed-form expression for Fibonacci numbers to achieve an O(1) time, O(1) space solution using Binet's formula.

### Code
```cpp
#include <cmath>

int climbStairs(int n) {
    // Golden ratio
    double sqrt5 = std::sqrt(5);
    double phi = (1 + sqrt5) / 2;
    double psi = (1 - sqrt5) / 2;
    
    // Using Binet's formula for Fibonacci numbers
    return std::round((std::pow(phi, n+1) - std::pow(psi, n+1)) / sqrt5);
}
```

### Complexity
- **Time Complexity:** O(1) - Constant time computations.
- **Space Complexity:** O(1) - No additional space usage apart from a few variables.

