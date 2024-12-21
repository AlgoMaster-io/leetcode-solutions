# [808. Soup Servings](https://leetcode.com/problems/soup-servings/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Dynamic Programming with Memoization](#dynamic-programming-with-memoization)

## Recursive Approach

### Intuition:
This problem can be solved by simulating the pouring operations starting from `A = N` and `B = N`. The base case is if either `A` or `B` becomes zero, which gives us the probability of either `A` being empty first, `B` being empty first, or both being empty at the same operation. We perform this recursively considering each of the four types of serving operations.

### Steps:

1. Define a recursive function `serve(A, B)` which returns the probability that A is empty first or both run out simultaneously.
2. Implement the base cases to return appropriate probabilities:
   - If `A <= 0` and `B <= 0`, return `0.5` (both A and B are empty).
   - If `A <= 0`, return `1.0` (A is empty first).
   - If `B <= 0`, return `0.0` (B is empty first).
3. For each of the four serving operations, recursively calculate the resulting probability and average them.
4. Initiate this process by calling `serve(N, N)`.

### Time Complexity:
The time complexity here is exponential, O(4^N) because of the four branching recursive calls for each operation.

### Space Complexity:
Space complexity is O(N) due to the recursion stack.

```cpp
class Solution {
public:
    double serve(int A, int B) {
        // Base cases
        if (A <= 0 && B <= 0) return 0.5; // Both A and B empty at the same time
        if (A <= 0) return 1.0;           // A is empty and B still has some servings left
        if (B <= 0) return 0.0;           // B is empty and A still has some servings left
        
        // Recursive calls for each serving option with 0.25 weight for each
        return 0.25 * (serve(A - 100, B) +
                       serve(A - 75, B - 25) +
                       serve(A - 50, B - 50) +
                       serve(A - 25, B - 75));
    }
    
    double soupServings(int N) {
        // Anything beyond a threshold, probability converges to 1
        if (N > 5000) return 1.0;
        // Running the recursive function
        return serve(N, N);
    }
};
```

## Dynamic Programming with Memoization

### Intuition:
The recursive approach has overlapping subproblems, which makes it a good candidate for memoization. By storing the results of computed states `(A, B)`, we can avoid redundant calculations and thereby improve efficiency.

### Steps:

1. Use a Map or vector of vector to store the results of already calculated states.
2. Modify the `serve` function to check and store results in the memoization table.
3. The rest of the approach remains the same as the recursive approach, using the table to retrieve already computed values.

### Time Complexity:
With memoization, the time complexity reduces significantly to approximately O(N^2) due to caching.

### Space Complexity:
Space complexity remains O(N^2) for storing computed results in the memoization table.

```cpp
class Solution {
public:
    std::unordered_map<int, std::unordered_map<int, double>> memo;
    
    double serve(int A, int B) {
        if (A <= 0 && B <= 0) return 0.5;
        if (A <= 0) return 1.0;
        if (B <= 0) return 0.0;
        
        // Check if results for (A, B) are already calculated
        if (memo[A][B] > 0) return memo[A][B];
        
        // Store the result in memo table
        memo[A][B] = 0.25 * (serve(A - 100, B) +
                             serve(A - 75, B - 25) +
                             serve(A - 50, B - 50) +
                             serve(A - 25, B - 75));
        
        return memo[A][B];
    }
    
    double soupServings(int N) {
        if (N > 5000) return 1.0;
        return serve(N, N);
    }
};
```

These solutions incrementally improve from basic recursion to a more optimized memoized approach, handling large inputs efficiently.

