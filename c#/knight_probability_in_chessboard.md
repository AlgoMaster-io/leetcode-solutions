# 688. [Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Approach 1: Recursion with Memoization

### Solution
csharp
```csharp
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2 * k)
public class Solution {
    private static readonly int[][] DIRECTIONS = new int[][] {
        new int[] {1, 2}, new int[] {1, -2}, new int[] {-1, 2}, new int[] {-1, -2},
        new int[] {2, 1}, new int[] {2, -1}, new int[] {-2, 1}, new int[] {-2, -1}
    };

    public double KnightProbability(int N, int K, int r, int c) {
        double[,,] memo = new double[N, N, K + 1];
        for (int i = 0; i < N; i++)
            for (int j = 0; j < N; j++)
                for (int k = 0; k <= K; k++)
                    memo[i, j, k] = -1;

        return FindProbability(N, K, r, c, memo) / Math.Pow(8, K);
    }

    private double FindProbability(int N, int K, int r, int c, double[,,] memo) {
        if (r < 0 || r >= N || c < 0 || c >= N) return 0;
        if (K == 0) return 1;
        
        if (memo[r, c, K] != -1) return memo[r, c, K];

        double prob = 0;
        foreach (var direction in DIRECTIONS) {
            int newRow = r + direction[0];
            int newCol = c + direction[1];
            prob += FindProbability(N, K - 1, newRow, newCol, memo);
        }

        memo[r, c, K] = prob;
        return prob;
    }
}
```

## Approach 2: Dynamic Programming (Bottom-Up)

### Solution
csharp
```csharp
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2)
public class Solution {
    private static readonly int[][] DIRECTIONS = new int[][] {
        new int[] {1, 2}, new int[] {1, -2}, new int[] {-1, 2}, new int[] {-1, -2},
        new int[] {2, 1}, new int[] {2, -1}, new int[] {-2, 1}, new int[] {-2, -1}
    };

    public double KnightProbability(int N, int K, int r, int c) {
        double[,] dp0 = new double[N, N];
        dp0[r, c] = 1;

        for (int step = 0; step < K; step++) {
            double[,] dp1 = new double[N, N];
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    if (dp0[i, j] > 0) {
                        foreach (var direction in DIRECTIONS) {
                            int ni = i + direction[0];
                            int nj = j + direction[1];
                            if (ni >= 0 && ni < N && nj >= 0 && nj < N) {
                                dp1[ni, nj] += dp0[i, j] / 8.0;
                            }
                        }
                    }
                }
            }
            dp0 = dp1;
        }

        double result = 0;
        foreach (var x in dp0) {
            result += x;
        }
        return result;
    }
}
```

## Approach 3: Space-Optimized Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2)
public class Solution {
    private static readonly int[][] DIRECTIONS = new int[][] {
        new int[] {1, 2}, new int[] {1, -2}, new int[] {-1, 2}, new int[] {-1, -2},
        new int[] {2, 1}, new int[] {2, -1}, new int[] {-2, 1}, new int[] {-2, -1}
    };

    public double KnightProbability(int N, int K, int r, int c) {
        double[,] current = new double[N, N];
        current[r, c] = 1;

        for (int step = 0; step < K; step++) {
            double[,] next = new double[N, N];
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    if (current[i, j] > 0) {
                        foreach (var direction in DIRECTIONS) {
                            int ni = i + direction[0];
                            int nj = j + direction[1];
                            if (ni >= 0 && ni < N && nj >= 0 && nj < N) {
                                next[ni, nj] += current[i, j] / 8.0;
                            }
                        }
                    }
                }
            }
            current = next;
        }

        double result = 0;
        foreach (var x in current) {
            result += x;
        }
        return result;
    }
}
```

