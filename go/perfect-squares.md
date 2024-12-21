# Leetcode Problem: [279. Perfect Squares](https://leetcode.com/problems/perfect-squares/)

## Navigation
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)
- [Approach 3: Breadth-First Search (BFS)](#approach-3-breadth-first-search-bfs)

## Approach 1: Brute Force

### Intuition
The simplest way to solve this problem is to recursively try and subtract perfect squares from `n` until you reach zero, counting the number of perfect squares you've subtracted. This is very inefficient, but it gives a good base understanding of how the problem can be approached.

### Implementation
```go
func numSquaresBruteForce(n int) int {
    return helper(n, int(math.Sqrt(float64(n))))
}

func helper(n, maxSqrt int) int {
    if n == 0 {
        return 0
    }
    minCount := n
    for i := maxSqrt; i >= 1; i-- {
        sq := i * i
        if n >= sq {
            // Recursively subtract squares and count each depth
            minCount = min(minCount, 1 + helper(n - sq, i))
        }
    }
    return minCount
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```
### Complexity Analysis
- **Time Complexity**: O((sqrt(n))^n) - Exponential due to recursion.
- **Space Complexity**: O(n) - Depth of the stack can go up to `n`.

## Approach 2: Dynamic Programming

### Intuition
The brute force solution involves a lot of repeated calculations — we can optimize it using dynamic programming. We'll keep track of the least number of perfect squares needed for each number from `1` to `n`, using prior results to build up to a solution efficiently.

### Implementation
```go
func numSquaresDP(n int) int {
    // dp[i] represents the least number of perfect square numbers which sum to i
    dp := make([]int, n + 1)
    for i := 1; i <= n; i++ {
        dp[i] = i // Maximum squares required is i (1^2 + 1^2 + ...)
        for j := 1; j * j <= i; j++ {
            dp[i] = min(dp[i], dp[i - j * j] + 1)
        }
    }
    return dp[n]
}
```
### Complexity Analysis
- **Time Complexity**: O(n * sqrt(n)) - For each number up to n, iterating through possible perfect squares.
- **Space Complexity**: O(n) - Due to the `dp` array.

## Approach 3: Breadth-First Search (BFS)

### Intuition
A different perspective of the problem is to think of it as finding the shortest path in an unweighted graph — where each number can be connected to the numbers derived by subtracting perfect squares. BFS perfectly suits such shortest path problems.

### Implementation
```go
func numSquaresBFS(n int) int {
    if n < 2 {
        return n
    }

    // Queue for BFS
    queue := []int{n}
    level := 0

    for len(queue) > 0 {
        level++
        nextQueue := []int{}
        for _, remainder := range queue {
            for i := 1; i * i <= remainder; i++ {
                next := remainder - i * i
                if next == 0 {
                    return level
                }
                nextQueue = append(nextQueue, next)
            }
        }
        queue = nextQueue
    }
    return level
}
```
### Complexity Analysis
- **Time Complexity**: O(n * sqrt(n)) - In the worst case, we explore each number `i` for possible square reductions up until `sqrt(i)`.
- **Space Complexity**: O(n) - Queue can grow up to the size of `n`.

