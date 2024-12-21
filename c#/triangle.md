# [Leetcode 120: Triangle](https://leetcode.com/problems/triangle/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Memoization (Top-Down) Approach](#memoization-top-down-approach)
3. [Dynamic Programming (Bottom-Up) Approach](#dynamic-programming-bottom-up-approach)

### Recursive Approach

#### Intuition:
The problem can be visualized as finding the minimum path sum from the top of the triangle to the base by moving only to adjacent numbers. The simplest idea is to use recursion to explore every path from the top to the bottom, summing the path values and keeping track of the minimum.

#### Solution:
We start from the top and at each level, recursively decide to move to the element directly below or the element diagonally below. We sum the values along the path and calculate the minimum path sum.

```csharp
public class Solution {
    public int MinimumTotal(IList<IList<int>> triangle) {
        return MinPathSum(triangle, 0, 0);
    }
    
    private int MinPathSum(IList<IList<int>> triangle, int level, int index) {
        // Base case: If we reach the last level of the triangle
        if (level == triangle.Count) return 0;
        
        // Recursive case: Minimum sum of paths by moving to adjacent nodes
        int left = MinPathSum(triangle, level + 1, index);
        int right = MinPathSum(triangle, level + 1, index + 1);
        
        // Return current node value plus the minimum path sum of both adjacent nodes
        return Math.Min(left, right) + triangle[level][index];
    }
}
```

#### Complexity:
- **Time Complexity**: O(2^n) - Since in each step we have two recursive calls (binary tree).
- **Space Complexity**: O(n) - Recursion stack.

### Memoization (Top-Down) Approach

#### Intuition:
We can optimize the recursive approach using memoization by storing the results of previously computed paths to avoid redundant calculations.

#### Solution:
We use a cache to store the results of paths already computed. This avoids recomputing results for the same subproblems.

```csharp
public class Solution {
    public int MinimumTotal(IList<IList<int>> triangle) {
        int[][] memo = new int[triangle.Count][];
        for (int i = 0; i < triangle.Count; i++) {
            memo[i] = new int[triangle[i].Count];
            Array.Fill(memo[i], int.MaxValue);
        }
        return MinPathSum(triangle, 0, 0, memo);
    }
    
    private int MinPathSum(IList<IList<int>> triangle, int level, int index, int[][] memo) {
        // Base case: If we reach the last level of the triangle
        if (level == triangle.Count) return 0;

        // Memoized result
        if (memo[level][index] != int.MaxValue) return memo[level][index];

        // Recursive case with memoization
        int left = MinPathSum(triangle, level + 1, index, memo);
        int right = MinPathSum(triangle, level + 1, index + 1, memo);

        // Cache the result
        memo[level][index] = Math.Min(left, right) + triangle[level][index];
        
        return memo[level][index];
    }
}
```

#### Complexity:
- **Time Complexity**: O(n^2) - Every subproblem is computed once.
- **Space Complexity**: O(n^2) - Memoization cache.

### Dynamic Programming (Bottom-Up) Approach

#### Intuition:
To avoid the complexities of recursion, we can build the solution iteratively from bottom up, modifying the triangle to store the minimum path sum at each level.

#### Solution:
We start from the second-to-last row and move upwards, updating each element to be the sum of itself and the minimum of its two possible children below it. 

```csharp
public class Solution {
    public int MinimumTotal(IList<IList<int>> triangle) {
        // Start from the second last row and move upwards
        for (int level = triangle.Count - 2; level >= 0; level--) {
            for (int index = 0; index < triangle[level].Count; index++) {
                // Update each value to be the minimum path sum
                triangle[level][index] += Math.Min(
                    triangle[level + 1][index], 
                    triangle[level + 1][index + 1]);
            }
        }
        
        // The top element contains the minimum path sum
        return triangle[0][0];
    }
}
```

#### Complexity:
- **Time Complexity**: O(n^2) - Updating the triangle in-place.
- **Space Complexity**: O(1) - No additional space used outside input.

This dynamic programming approach is the most optimal solution provided, allowing us to compute the minimum path sum with minimal memory usage and avoiding the overhead of recursion.

