# 279. [Perfect Squares](https://leetcode.com/problems/perfect-squares/)

## Approach 1: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n * sqrt(n))
// Space Complexity: O(n)
#include <vector>
#include <algorithm>
#include <climits>

class Solution {
public:
    int numSquares(int n) {
        std::vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0; // Zero can be represented by zero squares

        // Start filling dp array
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j * j <= i; j++) {
                dp[i] = std::min(dp[i], dp[i - j * j] + 1);
            }
        }

        return dp[n]; // Result is stored in dp[n]
    }
};
```

## Approach 2: Breadth-First Search (BFS)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <queue>
#include <vector>

class Solution {
public:
    int numSquares(int n) {
        std::queue<int> queue;
        std::vector<int> visited(n + 1, 0);
        queue.push(0);
        visited[0] = 1;

        int level = 0;
        while (!queue.empty()) {
            int size = queue.size();
            level++;
            for (int i = 0; i < size; i++) {
                int sum = queue.front();
                queue.pop();
                for (int j = 1; sum + j * j <= n; j++) {
                    int next = sum + j * j;
                    if (next == n) {
                        return level;
                    }
                    if (visited[next] == 0) {
                        queue.push(next);
                        visited[next] = 1;
                    }
                }
            }
        }

        return level;
    }
};
```

## Approach 3: Legendre's Three-Square Theorem

### Solution
```cpp
// Time Complexity: O(sqrt(n))
// Space Complexity: O(1)
#include <cmath>

class Solution {
public:
    int numSquares(int n) {
        // Check if n is a perfect square
        if (isPerfectSquare(n)) return 1;

        // Check the result is 4 using Legendre's Theorem
        while (n % 4 == 0) {
            n /= 4;
        }
        if (n % 8 == 7) return 4;

        // Check if it can be decomposed into sum of two squares
        for (int i = 1; i * i <= n; i++) {
            if (isPerfectSquare(n - i * i)) return 2;
        }

        return 3;
    }

private:
    bool isPerfectSquare(int n) {
        int s = static_cast<int>(std::sqrt(n));
        return s * s == n;
    }
};
```

