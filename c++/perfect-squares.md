# [Perfect Squares](https://leetcode.com/problems/perfect-squares/)

## Solutions:

- [Approach 1: Backtracking (Naive Solution)](#approach-1-backtracking-naive-solution)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)
- [Approach 3: Breadth-First Search (BFS)](#approach-3-breadth-first-search-bfs)

---

### Approach 1: Backtracking (Naive Solution)

#### Intuition:

This approach involves trying every possible combination of perfect squares that sum up to the given number `n`. It uses recursion and backtracking to explore all potential solutions.

1. Generate a list of perfect squares up to `n`.
2. Use recursive backtracking to find combinations of these perfect squares that sum up to `n`.
3. The depth of recursion is tracked, aiming to find the minimum depth where the sum equals `n`.

#### Detailed Comments:

```cpp
#include <iostream>
#include <vector>
#include <cmath>
#include <climits>

class Solution {
    int minCount;

    void backtrack(int n, int count, std::vector<int>& squares, int index) {
        // If n becomes zero, update the minimum count
        if (n == 0) {
            minCount = std::min(minCount, count);
            return;
        }
        // If n becomes negative, prune this path
        if (n < 0) return;

        // Try adding each square starting from the current index
        for (int i = index; i < squares.size(); ++i) {
            backtrack(n - squares[i], count + 1, squares, i);
        }
    }

public:
    int numSquares(int n) {
        // Generate perfect squares up to n
        std::vector<int> squares;
        for (int i = 1; i * i <= n; ++i) {
            squares.push_back(i * i);
        }

        minCount = INT_MAX;
        backtrack(n, 0, squares, 0);
        return minCount;
    }
};
```

**Time Complexity:** O(n^(n/2))  
**Space Complexity:** O(sqrt(n)), due to the list of perfect squares.

---

### Approach 2: Dynamic Programming

#### Intuition:

This approach leverages dynamic programming to build a solution in a bottom-up manner. We maintain an array `dp` where `dp[i]` represents the minimum number of perfect square numbers that sum to `i`.

1. Initialize an array `dp` of size `n + 1` with values `INT_MAX` (except `dp[0]` which is `0`).
2. For each integer from `1` to `n`, try reducing it by each perfect square number less than it, and check if we can reduce the number of perfect squares used.

#### Detailed Comments:

```cpp
#include <iostream>
#include <vector>
#include <cmath>

class Solution {
public:
    int numSquares(int n) {
        // dp array to store the least number of perfect square numbers for each number up to n
        std::vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0; // Base case

        // Compute dp values from 1 to n
        for (int i = 1; i <= n; ++i) {
            for (int j = 1; j * j <= i; ++j) {
                // Update dp[i] by checking possible perfect squares
                dp[i] = std::min(dp[i], dp[i - j * j] + 1);
            }
        }

        // dp[n] will contain the result
        return dp[n];
    }
};
```

**Time Complexity:** O(n * sqrt(n))  
**Space Complexity:** O(n), for the DP array.

---

### Approach 3: Breadth-First Search (BFS)

#### Intuition:

This approach models the problem as a graph where each node represents a remaining sum, and edges represent subtracting a perfect square. We use BFS to find the shortest path from `n` to `0`.

1. Use a queue to explore possible sums by subtracting perfect squares.
2. Each level in the BFS corresponds to using one more perfect square.
3. The first time we reach zero, we have found the minimum number of perfect squares.

#### Detailed Comments:

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <cmath>

class Solution {
public:
    int numSquares(int n) {
        // Queue for the BFS
        std::queue<std::pair<int, int>> q;
        q.push({n, 0});

        std::vector<bool> visited(n + 1, false);
        visited[n] = true;

        // BFS loop
        while (!q.empty()) {
            auto [current, depth] = q.front();
            q.pop();

            // Try subtracting each perfect square less than current
            for (int i = 1; i * i <= current; ++i) {
                int next = current - i * i;
                if (next == 0) return depth + 1; // Found a solution
                
                // If not visited, add to queue
                if (!visited[next]) {
                    q.push({next, depth + 1});
                    visited[next] = true;
                }
            }
        }
        return -1; // Should never reach here
    }
};
```

**Time Complexity:** O(n * sqrt(n))  
**Space Complexity:** O(n), due to the queue and visited array.

---

