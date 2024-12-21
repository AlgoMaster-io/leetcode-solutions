# [Leetcode Problem 1937: Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Dynamic Programming](#optimized-dynamic-programming)

---

## Brute Force Approach

### Intuition
The Brute Force approach for this problem involves considering every possible path through the matrix, calculating the cost for each path, and then returning the maximum cost found. This is achieved by iterating over the matrix in a row-wise manner, and for each cell in a given row, calculating the cost based on all possible previous cells from the row above, accounting for their respective penalties.

### Detailed Explanation
The naive way to solve this problem is to iterate over each row and for every cell in the current row, consider every possible previous cell in the row above. For each combination, we calculate the cost using the given formula, and we keep track of the maximum cost encountered thus far.

### Code
```java
public class Solution {
    public long maxPoints(int[][] points) {
        int m = points.length;
        int n = points[0].length;

        // dp array to hold the maximum points until the current row
        long[] dp = new long[n];
        
        // Initialize dp with the first row points
        for (int j = 0; j < n; j++) {
            dp[j] = points[0][j];
        }

        for (int i = 1; i < m; i++) {
            // Temporary array to store the new dp values for the current row
            long[] newDp = new long[n];
            for (int j = 0; j < n; j++) {
                // Look at every k from the previous row to calculate the maximum points
                for (int k = 0; k < n; k++) {
                    newDp[j] = Math.max(newDp[j], dp[k] + points[i][j] - Math.abs(j - k));
                }
            }
            dp = newDp;
        }

        long maxPoints = 0;
        for (long val : dp) {
            maxPoints = Math.max(maxPoints, val);
        }

        return maxPoints;
    }
}
```

### Complexity Analysis
- **Time Complexity**: \(O(m \times n^2)\), as we are iterating through each cell of the matrix and comparing it with every cell of the row above.
- **Space Complexity**: \(O(n)\), using a temporary array for dynamic programming storage.

---

## Optimized Dynamic Programming

### Intuition
We can optimize the brute-force solution by utilizing dynamic programming to progressively build up the solution. We will keep a record of the maximum costs achievable for each cell, using information from the previous row. The idea is to avoid recalculating penalties for non-optimal choices by using pre-computed maximum values from the left and right neighbors.

### Detailed Explanation
To optimize our solution, we can pass through the matrix twice for each row. The first pass calculates the maximum possible score for each cell considering penalties from the left neighbor direction. The second pass does the same from the right neighbor direction. Thus, each cell will have the maximum score via the least penalized neighbor direction. By maintaining efficient transitions between computations, the solution is achieved more rapidly than the naive method.

### Code
```java
public class Solution {
    public long maxPoints(int[][] points) {
        int m = points.length;
        int n = points[0].length;
        
        long[] dp = new long[n];
        
        // Initialize dp with the first row points
        for (int j = 0; j < n; j++) {
            dp[j] = points[0][j];
        }
        
        for (int i = 1; i < m; i++) {
            long[] newDp = new long[n];

            // Calculate the maximum from the left
            long[] leftMax = new long[n];
            leftMax[0] = dp[0];
            for (int j = 1; j < n; j++) {
                leftMax[j] = Math.max(leftMax[j - 1] - 1, dp[j]);
            }
            
            // Calculate the maximum from the right
            long[] rightMax = new long[n];
            rightMax[n - 1] = dp[n - 1];
            for (int j = n - 2; j >= 0; j--) {
                rightMax[j] = Math.max(rightMax[j + 1] - 1, dp[j]);
            }
            
            // Calculate the new dp based on leftMax, rightMax, and points
            for (int j = 0; j < n; j++) {
                newDp[j] = points[i][j] + Math.max(leftMax[j], rightMax[j]);
            }
            
            dp = newDp;
        }

        long maxPoints = 0;
        for (long val : dp) {
            maxPoints = Math.max(maxPoints, val);
        }
        
        return maxPoints;
    }
}
```

### Complexity Analysis
- **Time Complexity**: \(O(m \times n)\), a significant improvement since we take advantage of linear computations for each row.
- **Space Complexity**: \(O(n)\), as we use additional space for temporary storage in dynamic programming approach.

By following these strategies, you can handle the problem statement effectively, enhancing the performance of the solution from a brute-force method to an optimized approach leveraging dynamic programming.

