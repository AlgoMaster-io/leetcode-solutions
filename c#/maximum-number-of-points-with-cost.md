# [Leetcode 1937: Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming with Optimization for Prefix and Suffix Calculation](#approach-2-dynamic-programming-with-optimization-for-prefix-and-suffix-calculation)

### Approach 1: Brute Force

#### Intuition

In this approach, we consider each cell in the current row, calculate the maximum score by considering every possible cell from the previous row and account for the cost. For a cell in column `j`, the cost is determined by the absolute difference between the column indices `|j - k|` for a cell in column `k` of the previous row. This will give a clear, albeit inefficient, method to determine the maximum possible points at each cell by brute-forcing through all possibilities.

#### Steps

1. Initialize a 2D array `dp` where `dp[i][j]` represents the maximum points obtainable at row `i`, column `j`.
2. Set the first row in `dp` to be the same as that of `points` since no cost occurs before the first row.
3. For each row starting from the second, calculate the maximum possible points for each column.
4. Choose the maximum possible points for each cell by evaluating every possible previous column.

#### Code

```csharp
public class Solution {
    public long MaxPoints(int[][] points) {
        int m = points.Length, n = points[0].Length;
        long[,] dp = new long[m, n];

        // Initialize first row of dp with points
        for (int j = 0; j < n; j++) {
            dp[0, j] = points[0][j];
        }

        // Iterate over rows
        for (int i = 1; i < m; i++) {
            // Iterate over columns to compute max points for dp[i, j]
            for (int j = 0; j < n; j++) {
                long maxValue = 0;
                // Consider each cell in previous row
                for (int k = 0; k < n; k++) {
                    // Compute value considering cost
                    long value = dp[i-1, k] + points[i][j] - Math.Abs(k - j);
                    maxValue = Math.Max(maxValue, value);
                }
                dp[i, j] = maxValue;
            }
        }

        // The result is the maximum in the last row of dp
        long result = 0;
        for (int j = 0; j < n; j++) {
            result = Math.Max(result, dp[m-1, j]);
        }
        return result;
    }
}
```

#### Time Complexity
- **O(m * n^2):** We need to evaluate each cell with respect to all previous columns.

#### Space Complexity
- **O(m * n):** Space required to store the `dp` table.

### Approach 2: Dynamic Programming with Optimization for Prefix and Suffix Calculation

#### Intuition

The brute-force approach is computationally expensive as it attempts to incrementally build by exploring all possibilities without optimization. 

Instead, for each row, we build the solution using two auxiliary arrays, `left` and `right`, representing the accumulated left and right maximum values adjusted by the cost incurred from transitioning from row `i-1` to `i`. This allows calculating the maximum achievable points by considering optimal previous positions efficiently.

#### Steps

1. Use a `dp` array for the current state only to optimize space.
2. Utilize two arrays `left` and `right` for tracking the max values from the left and right.
3. Calculate `left` by moving from left to right, while accumulating max possible scores.
4. Calculate `right` by moving from right to left, while accumulating max possible scores.

#### Code

```csharp
public class Solution {
    public long MaxPoints(int[][] points) {
        int m = points.Length, n = points[0].Length;
        long[] dp = new long[n];
        
        // Initial dp setup with first row
        for (int j = 0; j < n; j++) {
            dp[j] = points[0][j];
        }

        // Iterate over each row starting from the second
        for (int i = 1; i < m; i++) {
            long[] left = new long[n];
            long[] right = new long[n];
            long[] new_dp = new long[n];

            // Process left max values
            left[0] = dp[0];
            for (int j = 1; j < n; j++) {
                left[j] = Math.Max(left[j - 1], dp[j] + j);
            }

            // Process right max values
            right[n - 1] = dp[n - 1] - (n - 1);
            for (int j = n - 2; j >= 0; j--) {
                right[j] = Math.Max(right[j + 1], dp[j] - j);
            }

            // Calculate the new dp for the current row using optimized previous row values
            for (int j = 0; j < n; j++) {
                new_dp[j] = Math.Max(left[j] - j, right[j] + j) + points[i][j];
            }

            // Update dp for next iteration
            dp = new_dp;
        }

        // Find the maximum value from the last calculated dp
        long result = dp.Max();
        return result;
    }
}
```

#### Time Complexity
- **O(m * n):** Efficient linear computation for each row via optimized space allocation.

#### Space Complexity
- **O(n):** Reduced space with current `dp`, `left`, and `right` arrays.

In conclusion, Approach 1 does a comprehensive cross-examination of all possibilities with a high computational cost, while Approach 2 utilizes mathematical insight to simplify comparisons in a row-linear manner, presenting a significant optimization.

