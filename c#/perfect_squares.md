# 279. [Perfect Squares](https://leetcode.com/problems/perfect-squares/)

## Approach 1: Dynamic Programming

### Solution
```csharp
// Time Complexity: O(n * sqrt(n))
// Space Complexity: O(n)
public class Solution {
    public int NumSquares(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 0; // Zero can be represented by zero squares
        
        // Start filling dp array
        for (int i = 1; i <= n; i++) {
            int min = int.MaxValue;
            for (int j = 1; j * j <= i; j++) {
                min = Math.Min(min, dp[i - j * j] + 1);
            }
            dp[i] = min;
        }
        
        return dp[n]; // Result is stored in dp[n]
    }
}
```

## Approach 2: Breadth-First Search (BFS)

### Solution
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int NumSquares(int n) {
        Queue<int> queue = new Queue<int>();
        int[] visited = new int[n + 1];
        queue.Enqueue(0);
        visited[0] = 1;
        
        int level = 0;
        while (queue.Count > 0) {
            int size = queue.Count;
            level++;
            for (int i = 0; i < size; i++) {
                int sum = queue.Dequeue();
                for (int j = 1; sum + j * j <= n; j++) {
                    int next = sum + j * j;
                    if (next == n) {
                        return level;
                    }
                    if (visited[next] == 0) {
                        queue.Enqueue(next);
                        visited[next] = 1;
                    }
                }
            }
        }
        
        return level;
    }
}
```

## Approach 3: Legendre's Three-Square Theorem

### Solution
```csharp
// Time Complexity: O(sqrt(n))
// Space Complexity: O(1)
public class Solution {
    public int NumSquares(int n) {
        // Check if n is a perfect square
        if (IsPerfectSquare(n)) return 1;

        // Check the result is 4 using Legendre's Theorem
        while (n % 4 == 0) { 
            n /= 4; 
        }
        if (n % 8 == 7) return 4;

        // Check if it can be decomposed into sum of two squares
        for (int i = 1; i * i <= n; i++) {
            if (IsPerfectSquare(n - i * i)) return 2;
        }

        return 3;
    }

    private bool IsPerfectSquare(int n) {
        int s = (int) Math.Sqrt(n);
        return s * s == n;
    }
}
```

