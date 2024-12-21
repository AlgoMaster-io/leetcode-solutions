# [Leetcode 688: Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Navigation
- [Approach 1: Brute Force Recursion](#approach-1)
- [Approach 2: Dynamic Programming](#approach-2)
- [Approach 3: Optimized Dynamic Programming with Memoization](#approach-3)

---

## Approach 1: Brute Force Recursion

### Intuition
This problem can be thought of as a tree of probabilities. At every step, the knight can move to 8 possible positions. We need to recursively calculate the probability of staying within bounds for every move, starting from the initial position.

### Implementation

```cpp
class Solution {
private:
    // Directions a knight can move in a chessboard
    vector<pair<int, int>> directions = {
        {2, 1}, {2, -1}, {-2, 1}, {-2, -1},
        {1, 2}, {1, -2}, {-1, 2}, {-1, -2}
    };

    // Recursive function to calculate probabilities
    double dfs(int n, int k, int row, int col) {
        // If the knight is out of bounds, return 0
        if (row < 0 || col < 0 || row >= n || col >= n) {
            return 0.0;
        } 
        // If no more moves are left, the probability is 1
        if (k == 0) {
            return 1.0;
        }
        
        double stayProbability = 0.0;
        // Calculate probability for each possible move
        for (auto& [dr, dc] : directions) {
            stayProbability += dfs(n, k - 1, row + dr, col + dc) / 8.0;
        }
        
        return stayProbability;
    }

public:
    double knightProbability(int n, int k, int row, int column) {
        return dfs(n, k, row, column);
    }
};
```

### Time Complexity
- Exponential time complexity: O(8^k), as each move results in 8 new states.

### Space Complexity
- Space complexity: O(k), considering recursion depth.

## Approach 2: Dynamic Programming

### Intuition
To optimize the brute force approach, we can use dynamic programming. This approach involves calculating probabilities for each possible position for each number of moves and storing these results for reuse.

### Implementation

```cpp
class Solution {
public:
    double knightProbability(int n, int k, int row, int column) {
        vector<vector<vector<double>>> dp(k + 1, vector<vector<double>>(n, vector<double>(n, 0.0)));
        
        vector<pair<int, int>> directions = {
            {2, 1}, {2, -1}, {-2, 1}, {-2, -1},
            {1, 2}, {1, -2}, {-1, 2}, {-1, -2}
        };
        
        // Initial probability at (row, column) without any moves is 1
        dp[0][row][column] = 1.0;
        
        for (int move = 1; move <= k; ++move) {
            for (int r = 0; r < n; ++r) {
                for (int c = 0; c < n; ++c) {
                    for (auto& [dr, dc] : directions) {
                        int prevRow = r + dr;
                        int prevCol = c + dc;
                        if (prevRow >= 0 && prevCol >= 0 && prevRow < n && prevCol < n) {
                            dp[move][r][c] += dp[move - 1][prevRow][prevCol] / 8.0;
                        }
                    }
                }
            }
        }
        
        double totalProbability = 0.0;
        // Sum up all probabilities for the last move
        for (int r = 0; r < n; ++r) {
            for (int c = 0; c < n; ++c) {
                totalProbability += dp[k][r][c];
            }
        }
        
        return totalProbability;
    }
};
```

### Time Complexity
- Time complexity: O(k * n^2), where k is the number of moves and n^2 for each cell on the board.

### Space Complexity
- Space complexity: O(k * n^2), to store the DP table.

## Approach 3: Optimized Dynamic Programming with Memoization

### Intuition
This solution reuses previously computed probabilities with an approach that reduces space usage by utilizing a single 2D array instead of a 3D array.

### Implementation

```cpp
class Solution {
public:
    double knightProbability(int n, int k, int row, int column) {
        vector<vector<double>> dp(n, vector<double>(n, 0.0));
        dp[row][column] = 1.0;
        
        vector<pair<int, int>> directions = {
            {2, 1}, {2, -1}, {-2, 1}, {-2, -1}, 
            {1, 2}, {1, -2}, {-1, 2}, {-1, -2}
        };

        for (int move = 0; move < k; ++move) {
            vector<vector<double>> newDp(n, vector<double>(n, 0.0));
            for (int r = 0; r < n; ++r) {
                for (int c = 0; c < n; ++c) {
                    if (dp[r][c] > 0) {
                        for (auto& [dr, dc] : directions) {
                            int newRow = r + dr;
                            int newCol = c + dc;
                            if (newRow >= 0 && newCol >= 0 && newRow < n && newCol < n) {
                                newDp[newRow][newCol] += dp[r][c] / 8.0;
                            }
                        }
                    }
                }
            }
            dp = move(newDp); // Keep only current move's probabilities
        }
        
        double totalProbability = 0.0;
        for (int r = 0; r < n; ++r) {
            for (int c = 0; c < n; ++c) {
                totalProbability += dp[r][c];
            }
        }
        
        return totalProbability;
    }
};
```

### Time Complexity
- Time complexity: O(k * n^2).

### Space Complexity
- Space complexity: O(n^2), which is more efficient as we only need to store two layers of the dp matrix at any time.

