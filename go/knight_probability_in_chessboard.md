# 688. [Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Approach 1: Recursion with Memoization

### Solution
```go
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2 * k)
package main

import "math"

var directions = [][]int{
    {1, 2}, {1, -2}, {-1, 2}, {-1, -2},
    {2, 1}, {2, -1}, {-2, 1}, {-2, -1},
}

func knightProbability(N int, K int, r int, c int) float64 {
    memo := make([][][]float64, N)
    for i := range memo {
        memo[i] = make([][]float64, N)
        for j := range memo[i] {
            memo[i][j] = make([]float64, K+1)
            for k := range memo[i][j] {
                memo[i][j][k] = -1
            }
        }
    }
    return findProbability(N, K, r, c, &memo) / math.Pow(8, float64(K))
}

func findProbability(N int, K int, r int, c int, memo *[][][]float64) float64 {
    if r < 0 || r >= N || c < 0 || c >= N {
        return 0
    }
    if K == 0 {
        return 1
    }
    if (*memo)[r][c][K] != -1 {
        return (*memo)[r][c][K]
    }
    prob := 0.0
    for _, direction := range directions {
        newRow := r + direction[0]
        newCol := c + direction[1]
        prob += findProbability(N, K-1, newRow, newCol, memo)
    }
    (*memo)[r][c][K] = prob
    return prob
}
```

## Approach 2: Dynamic Programming (Bottom-Up)

### Solution
```go
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2)
package main

var directions = [][]int{
    {1, 2}, {1, -2}, {-1, 2}, {-1, -2},
    {2, 1}, {2, -1}, {-2, 1}, {-2, -1},
}

func knightProbability(N int, K int, r int, c int) float64 {
    dp0 := make([][]float64, N)
    for i := range dp0 {
        dp0[i] = make([]float64, N)
    }
    dp0[r][c] = 1

    for step := 0; step < K; step++ {
        dp1 := make([][]float64, N)
        for i := range dp1 {
            dp1[i] = make([]float64, N)
        }
        for i := 0; i < N; i++ {
            for j := 0; j < N; j++ {
                if dp0[i][j] > 0 {
                    for _, direction := range directions {
                        ni := i + direction[0]
                        nj := j + direction[1]
                        if ni >= 0 && ni < N && nj >= 0 && nj < N {
                            dp1[ni][nj] += dp0[i][j] / 8.0
                        }
                    }
                }
            }
        }
        dp0 = dp1
    }

    result := 0.0
    for _, row := range dp0 {
        for _, x := range row {
            result += x
        }
    }
    return result
}
```

## Approach 3: Space-Optimized Dynamic Programming

### Solution
```go
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2)
package main

var directions = [][]int{
    {1, 2}, {1, -2}, {-1, 2}, {-1, -2},
    {2, 1}, {2, -1}, {-2, 1}, {-2, -1},
}

func knightProbability(N int, K int, r int, c int) float64 {
    current := make([][]float64, N)
    for i := range current {
        current[i] = make([]float64, N)
    }
    current[r][c] = 1

    for step := 0; step < K; step++ {
        next := make([][]float64, N)
        for i := range next {
            next[i] = make([]float64, N)
        }
        for i := 0; i < N; i++ {
            for j := 0; j < N; j++ {
                if current[i][j] > 0 {
                    for _, direction := range directions {
                        ni := i + direction[0]
                        nj := j + direction[1]
                        if ni >= 0 && ni < N && nj >= 0 && nj < N {
                            next[ni][nj] += current[i][j] / 8.0
                        }
                    }
                }
            }
        }
        current = next
    }

    result := 0.0
    for _, row := range current {
        for _, x := range row {
            result += x
        }
    }
    return result
}
```

