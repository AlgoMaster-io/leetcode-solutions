# [Leetcode 120: Triangle](https://leetcode.com/problems/triangle/)

## Approaches

- [Approach 1: Recursive Brute Force](#approach-1-recursive-brute-force)
- [Approach 2: Memoization](#approach-2-memoization)
- [Approach 3: Bottom-Up Dynamic Programming](#approach-3-bottom-up-dynamic-programming)
- [Approach 4: Space Optimized Dynamic Programming](#approach-4-space-optimized-dynamic-programming)

### Approach 1: Recursive Brute Force

**Intuition**:  
Start from the top of the triangle, at each step, move to one of the two adjacent numbers on the row below. Sum the paths and return the minimum path sum recursively.

```java
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        return helper(triangle, 0, 0);
    }

    // Recursive helper function to find minimum path sum
    private int helper(List<List<Integer>> triangle, int row, int col) {
        // Base case: If we are at the last row, return the current element
        if (row == triangle.size() - 1) {
            return triangle.get(row).get(col);
        }
        
        // Calculate the minimum path sum including the current element
        int leftPath = helper(triangle, row + 1, col);
        int rightPath = helper(triangle, row + 1, col + 1);
        
        return triangle.get(row).get(col) + Math.min(leftPath, rightPath);
    }
}
```

- **Time Complexity**: O(2^n) - each element can lead to two possibilities.
- **Space Complexity**: O(n) - recursion depth.

### Approach 2: Memoization

**Intuition**:  
To avoid recalculating the same values, we store the results of subproblems in a memoization table.

```java
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        // Initialize a memoization array with -1 (indicating uncomputed)
        int[][] memo = new int[triangle.size()][triangle.size()];
        for (int[] row : memo) {
            Arrays.fill(row, -1);
        }
        return helper(triangle, 0, 0, memo);
    }

    private int helper(List<List<Integer>> triangle, int row, int col, int[][] memo) {
        // Base case
        if (row == triangle.size() - 1) {
            return triangle.get(row).get(col);
        }

        // If the result is already computed, return it
        if (memo[row][col] != -1) {
            return memo[row][col];
        }

        // Calculate the minimum path sum using memoization
        int leftPath = helper(triangle, row + 1, col, memo);
        int rightPath = helper(triangle, row + 1, col + 1, memo);

        // Store the result in the memo array
        memo[row][col] = triangle.get(row).get(col) + Math.min(leftPath, rightPath);
        return memo[row][col];
    }
}
```

- **Time Complexity**: O(n^2) - all cells are computed once.
- **Space Complexity**: O(n^2) - memoization table storage.

### Approach 3: Bottom-Up Dynamic Programming

**Intuition**:  
Start filling the DP table from the bottom of the triangle towards the top, by considering the potential next moves in the row below.

```java
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[][] dp = new int[n][n];

        // Initialize the last row of DP table
        for (int i = 0; i < n; i++) {
            dp[n - 1][i] = triangle.get(n - 1).get(i);
        }

        // Fill the DP table from bottom to top
        for (int row = n - 2; row >= 0; row--) {
            for (int col = 0; col <= row; col++) {
                dp[row][col] = triangle.get(row).get(col) + Math.min(dp[row + 1][col], dp[row + 1][col + 1]);
            }
        }

        // The answer is at the top of the triangle
        return dp[0][0];
    }
}
```

- **Time Complexity**: O(n^2) - each cell is computed once.
- **Space Complexity**: O(n^2) - DP table storage.

### Approach 4: Space Optimized Dynamic Programming

**Intuition**:  
Instead of keeping a full DP table, keep a single array representing the current row computations, reducing space usage.

```java
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[] dp = new int[n];

        // Initialize the dp array with the last row of the triangle
        for (int i = 0; i < n; i++) {
            dp[i] = triangle.get(n - 1).get(i);
        }

        // Update dp array from bottom to top
        for (int row = n - 2; row >= 0; row--) {
            for (int col = 0; col <= row; col++) {
                dp[col] = triangle.get(row).get(col) + Math.min(dp[col], dp[col + 1]);
            }
        }

        // The answer is at the first element representing the top of the triangle
        return dp[0];
    }
}
```

- **Time Complexity**: O(n^2) - each cell is computed once.
- **Space Complexity**: O(n) - reused single array storage.

