# [Perfect Squares - LeetCode](https://leetcode.com/problems/perfect-squares/)

## Approaches:
1. [Breadth-First Search (BFS)](#breadth-first-search-bfs)
2. [Dynamic Programming](#dynamic-programming)

---

### Breadth-First Search (BFS)

#### Intuition:
The goal is to determine the least number of perfect square numbers that sum up to `n`. Breadth-First Search (BFS) can be utilized to explore the shortest path (minimum number of squares) to reach the sum `n` starting from zero.

In the BFS approach, we treat perfect square numbers as nodes and the sum of such numbers leading to `n` as paths between nodes. The problem boils down to finding the shortest path from zero to `n`.

#### Solution:

```csharp
public class Solution {
    public int NumSquares(int n) {
        // Queue for BFS
        Queue<int> queue = new Queue<int>();
        queue.Enqueue(n);

        // Level of BFS
        int level = 0;

        while (queue.Count > 0) {
            level++;
            int size = queue.Count;
            for (int i = 0; i < size; i++) {
                int remainder = queue.Dequeue();
                // Try all smaller squares
                for (int j = 1; j * j <= remainder; j++) {
                    int nextValue = remainder - j * j;
                    if (nextValue == 0) {
                        return level; // Found the answer
                    }
                    queue.Enqueue(nextValue);
                }
            }
        }
        return level;
    }
}
```

#### Time Complexity:
- **O(n * sqrt(n))**: Where `sqrt(n)` represents the number of perfect squares up to `n`, and potentially `O(n)` iterations to find the sum.
  
#### Space Complexity:
- **O(n)**: The size of the queue can grow up to `n`, in the worst case storing all numbers from `n` to zero.

---

### Dynamic Programming

#### Intuition:
Dynamic Programming (DP) offers an optimal way to solve this by storing results of previously solved sub-problems and building up to the solution of `n`. The DP state `dp[i]` will represent the minimum number of perfect squares that sum to `i`.

Starting with the base case where `dp[0] = 0` (zero requires zero numbers), we build up solutions for all numbers up to `n` by choosing the best possible combination of previously computed DP states and perfect square numbers.

#### Solution:

```csharp
public class Solution {
    public int NumSquares(int n) {
        // DP array with default values to infinity
        int[] dp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            dp[i] = int.MaxValue;
        }
        // Base case
        dp[0] = 0;

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j * j <= i; j++) {
                // Choose the smaller step possible to reach i
                dp[i] = Math.Min(dp[i], dp[i - j * j] + 1);
            }
        }

        return dp[n];
    }
}
```

#### Time Complexity:
- **O(n * sqrt(n))**: We are iterating over `n` and for each `n`, iterating through its potential perfect square roots.

#### Space Complexity:
- **O(n)**: To store the DP array with solutions from `0` to `n`.

