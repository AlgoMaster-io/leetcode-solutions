# [Leetcode 808: Soup Servings](https://leetcode.com/problems/soup-servings/)

## Solutions
1. [Recursive Approach with Memoization](#recursive-approach-with-memoization)
2. [Dynamic Programming Solution](#dynamic-programming-solution)
3. [Optimized Mathematical Approach](#optimized-mathematical-approach)

---

## Recursive Approach with Memoization

### Intuition
This problem involves serving units of soup A and soup B. The operations have an equal probability of reducing different volumes of soups. Our goal is to find the probability that soup A will be empty first. We will use a recursive function to simulate the serving process, and optimize it with memoization to avoid redundant calculations.

### Approach
- Use a recursive function `serve(A, B)` to model the serving process.
- For each call, consider the probability of the next four outcomes.
- The base cases are when A or B is empty. 
- Use memoization to store the results of `serve(A, B)` to prevent recalculating.

### Time and Space Complexity
- **Time Complexity:** `O(N^2)` - Each state (A,B) is computed at most once.
- **Space Complexity:** `O(N^2)` - Due to memoization storage.

```java
class Solution {
    private Map<String, Double> memo;
    
    public double soupServings(int N) {
        if (N > 4800) return 1; // approximation for large N
        this.memo = new HashMap<>();
        return serve(N, N);
    }
    
    private double serve(int A, int B) {
        if (A <= 0 && B <= 0) return 0.5; // Both soups get empty
        if (A <= 0) return 1.0; // A gets empty first
        if (B <= 0) return 0.0; // B gets empty first
        
        String key = A + "," + B;
        if (memo.containsKey(key)) return memo.get(key);
        
        double probability = 0.25 * (
            serve(A - 100, B) + // Operation 1
            serve(A - 75, B - 25) + // Operation 2
            serve(A - 50, B - 50) + // Operation 3
            serve(A - 25, B - 75) // Operation 4
        );
        
        memo.put(key, probability);
        return probability;
    }
}
```

---

## Dynamic Programming Solution

### Intuition
To improve upon the recursive approach, we can instead use dynamic programming. By iteratively building solutions for smaller subproblems, we can avoid recursion altogether.

### Approach
- Use a 2-D table `dp[i][j]` where `dp[i][j]` is the probability that when A = i and B = j, soup A will run out first.
- Fill this table by considering the outcomes of serving operations.
- Start from base cases and build up to (N, N).

### Time and Space Complexity
- **Time Complexity:** `O(N^2)`
- **Space Complexity:** `O(N^2)`

```java
class Solution {
    public double soupServings(int N) {
        if (N > 4800) return 1;
        N = (int)Math.ceil(N / 25.0);

        double[][] dp = new double[N + 1][N + 1];
        
        for (int i = 0; i <= N; i++) {
            for (int j = 0; j <= N; j++) {
                if (i == 0 && j == 0) {
                    dp[i][j] = 0.5;
                } else if (i == 0) {
                    dp[i][j] = 1.0;
                } else if (j == 0) {
                    dp[i][j] = 0.0;
                } else {
                    dp[i][j] = 0.25 * (
                        dp[Math.max(i-4, 0)][j] +
                        dp[Math.max(i-3, 0)][Math.max(j-1, 0)] +
                        dp[Math.max(i-2, 0)][Math.max(j-2, 0)] +
                        dp[Math.max(i-1, 0)][Math.max(j-3, 0)]
                    );
                }
            }
        }
        
        return dp[N][N];
    }
}
```

---

## Optimized Mathematical Approach

### Intuition
When `N` becomes very large, the probability approaches 1 due to constraints of problem dynamics. We can mathematically approximate the result to improve efficiency.

### Approach
- For a sufficiently large N (found empirically to be around 4800), the probability that soup A will run out first is virtually 1.
- For smaller N, use the DP solution as described.

### Time and Space Complexity
- **Time Complexity:** `O(1)` for large N, else `O(N^2)`
- **Space Complexity:** `O(1)` for large N, else `O(N^2)`

```java
class Solution {
    public double soupServings(int N) {
        if (N > 4800) return 1.0; // Approximation for larger N
        N = (int)Math.ceil(N / 25.0);

        double[][] dp = new double[N + 1][N + 1];
        
        for (int i = 0; i <= N; i++) {
            for (int j = 0; j <= N; j++) {
                if (i == 0 && j == 0) {
                    dp[i][j] = 0.5;
                } else if (i == 0) {
                    dp[i][j] = 1.0;
                } else if (j == 0) {
                    dp[i][j] = 0.0;
                } else {
                    dp[i][j] = 0.25 * (
                        dp[Math.max(i-4, 0)][j] +
                        dp[Math.max(i-3, 0)][Math.max(j-1, 0)] +
                        dp[Math.max(i-2, 0)][Math.max(j-2, 0)] +
                        dp[Math.max(i-1, 0)][Math.max(j-3, 0)]
                    );
                }
            }
        }
        
        return dp[N][N];
    }
}
```

