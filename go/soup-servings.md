# [Leetcode 808: Soup Servings](https://leetcode.com/problems/soup-servings/)

## Approaches
1. [Recursive Approach with Memoization](#recursive-approach-with-memoization)
2. [Iterative Dynamic Programming](#iterative-dynamic-programming)

---

## Recursive Approach with Memoization

### Intuition:
The problem is essentially a probability one, where we're trying to compute the probability that Soup A exhausts before Soup B. We start with a certain amount of Soup A and Soup B and serve them in specific quantities awaiting the first soup to empty.

To solve this recursively, we observe that in each operation, we have four possible ways to serve the soup. Our goal is to determine the probability of A serving before B, or both A and B serving at the same time (tie). We then memoize the result to avoid recalculating it.

### Approach:

1. Define a function `probability(a, b)` which returns the probability of serving soup A before B given `a` and `b` servings left respectively.

2. Base cases:
   - If `a <= 0` and `b <= 0`, return 0.5 (since both depleted at the same time, it's a tie).
   - If `a <= 0`, return 1.0 (since A depleted before B).
   - If `b <= 0`, return 0.0 (since B depleted before A).

3. Recursive case:
   - Since there are 4 ways to serve, calculate the probability by serving (100, 0), (75, 25), (50, 50), and (25, 75), recursively adding their probabilities and dividing by 4 (to account for all possibilities).

4. Use a dictionary to store already computed probabilities.

### Time Complexity:
- The time complexity is O(N^2) due to the number of unique states being computed (N/25)^2.

### Space Complexity:
- The space complexity is O(N^2) for the memoization dictionary.

Here's how you can implement this in Go:

```go
func soupServings(N int) float64 {
    if N > 5000 {
        return 1.0
    }
    memo := make(map[string]float64)
    
    var probability func(int, int) float64
    probability = func(a, b int) float64 {
        if a <= 0 && b <= 0 {
            return 0.5
        }
        if a <= 0 {
            return 1.0
        }
        if b <= 0 {
            return 0.0
        }
        
        key := fmt.Sprintf("%d_%d", a, b)
        if v, ok := memo[key]; ok {
            return v
        }
        
        memo[key] = 0.25 * (
            probability(a-100, b) +
            probability(a-75, b-25) +
            probability(a-50, b-50) +
            probability(a-25, b-75))
        
        return memo[key]
    }
    
    return probability(N, N)
}
```

---

## Iterative Dynamic Programming

### Intuition:
Instead of using memoization, we can fill a table to store the probabilities of soup portions. This allows us to iteratively compute the probabilities using dynamic programming.

### Approach:

1. Create a table `dp` where `dp[i][j]` represents the probability of soup A emptying before B given `i`, `j` servings left.

2. Initialize base cases similarly to the recursive approach.

3. Fill the table iteratively by computing each value using the previously computed states.

4. The result will be in `dp[N][N]`.

### Time Complexity:
- The time complexity is O(N^2), given the need to fill a table with (N/25)^2 entries.

### Space Complexity:
- The space complexity is O(N^2) for maintaining the DP table.

Here's how you can implement the iterative DP solution:

```go
func soupServingsIterative(N int) float64 {
    if N > 5000 {
        return 1.0
    }
    N = (N + 24) / 25
    dp := make([][]float64, N+1)
    for i := range dp {
        dp[i] = make([]float64, N+1)
    }
    
    dp[0][0] = 0.5
    for i := 1; i <= N; i++ {
        dp[0][i] = 1.0
    }
    
    for i := 1; i <= N; i++ {
        for j := 1; j <= N; j++ {
            dp[i][j] = 0.25 * (
                dp[max(i-4, 0)][j] +
                dp[max(i-3, 0)][max(j-1, 0)] +
                dp[max(i-2, 0)][max(j-2, 0)] +
                dp[max(i-1, 0)][max(j-3, 0)])
        }
    }
    
    return dp[N][N]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

This solution avoids recursive function calls and provides an iterative approach to solve the problem with dynamic programming. The time and space complexities match the recursive solution using memoization.

