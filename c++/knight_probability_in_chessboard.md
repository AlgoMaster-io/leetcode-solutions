# 688. [Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Approach 1: Recursion with Memoization

### Solution
```cpp
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2 * k)
class Solution {
    private:
        const vector<vector<int>> DIRECTIONS = {
            {1, 2}, {1, -2}, {-1, 2}, {-1, -2},
            {2, 1}, {2, -1}, {-2, 1}, {-2, -1}
        };

    public:
        double knightProbability(int N, int K, int r, int c) {
            vector<vector<vector<double>>> memo(N, vector<vector<double>>(N, vector<double>(K + 1, -1)));
            return findProbability(N, K, r, c, memo) / pow(8, K);
        }

    private:
        double findProbability(int N, int K, int r, int c, vector<vector<vector<double>>>& memo) {
            if (r < 0 || r >= N || c < 0 || c >= N) return 0;
            if (K == 0) return 1;
            if (memo[r][c][K] != -1) return memo[r][c][K];

            double prob = 0;
            for (const auto& direction : DIRECTIONS) {
                int newRow = r + direction[0];
                int newCol = c + direction[1];
                prob += findProbability(N, K - 1, newRow, newCol, memo);
            }

            memo[r][c][K] = prob;
            return prob;
        }
};
```

## Approach 2: Dynamic Programming (Bottom-Up)

### Solution
```cpp
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2)
class Solution {
    private:
        const vector<vector<int>> DIRECTIONS = {
            {1, 2}, {1, -2}, {-1, 2}, {-1, -2},
            {2, 1}, {2, -1}, {-2, 1}, {-2, -1}
        };

    public:
        double knightProbability(int N, int K, int r, int c) {
            vector<vector<double>> dp0(N, vector<double>(N, 0));
            dp0[r][c] = 1;

            for (int step = 0; step < K; step++) {
                vector<vector<double>> dp1(N, vector<double>(N, 0));
                for (int i = 0; i < N; i++) {
                    for (int j = 0; j < N; j++) {
                        if (dp0[i][j] > 0) {
                            for (const auto& direction : DIRECTIONS) {
                                int ni = i + direction[0];
                                int nj = j + direction[1];
                                if (ni >= 0 && ni < N && nj >= 0 && nj < N) {
                                    dp1[ni][nj] += dp0[i][j] / 8.0;
                                }
                            }
                        }
                    }
                }
                swap(dp0, dp1);
            }

            double result = 0;
            for (const auto& row : dp0) {
                for (double x : row) {
                    result += x;
                }
            }
            return result;
        }
};
```

## Approach 3: Space-Optimized Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n^2 * k)
// Space Complexity: O(n^2)
class Solution {
    private:
        const vector<vector<int>> DIRECTIONS = {
            {1, 2}, {1, -2}, {-1, 2}, {-1, -2},
            {2, 1}, {2, -1}, {-2, 1}, {-2, -1}
        };

    public:
        double knightProbability(int N, int K, int r, int c) {
            vector<vector<double>> current(N, vector<double>(N, 0));
            current[r][c] = 1;

            for (int step = 0; step < K; step++) {
                vector<vector<double>> next(N, vector<double>(N, 0));
                for (int i = 0; i < N; i++) {
                    for (int j = 0; j < N; j++) {
                        if (current[i][j] > 0) {
                            for (const auto& direction : DIRECTIONS) {
                                int ni = i + direction[0];
                                int nj = j + direction[1];
                                if (ni >= 0 && ni < N && nj >= 0 && nj < N) {
                                    next[ni][nj] += current[i][j] / 8.0;
                                }
                            }
                        }
                    }
                }
                swap(current, next);
            }

            double result = 0;
            for (const auto& row : current) {
                for (double x : row) {
                    result += x;
                }
            }
            return result;
        }
};
```

