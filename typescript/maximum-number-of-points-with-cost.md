# [Problem: 1937. Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost/)

## Approaches:
1. [Dynamic Programming with O(N^3) Complexity](#approach-1)
2. [Dynamic Programming with Pre-calculation O(N^2) Complexity](#approach-2)

### Approach 1: Dynamic Programming with O(N^3) Complexity

**Intuition**:

This problem can be seen as a variant of the classic "paint house" problem, but extended to two-dimensional space. We need to calculate the maximum points for each cell by considering moving from any cell in the previous row and subtracting the absolute column index difference as a cost.

**Approach**:

1. Use dynamic programming where `dp[i][j]` represents the maximum number of points obtainable at cell `(i, j)`.
2. Initialize the first row `dp[0][j]` with the values from `points[0][j]` because no previous rows exist.
3. For each cell `(i, j)`, calculate `dp[i][j]` as the maximum of `dp[i-1][k] + points[i][j] - abs(k - j)`, where `0 <= k < n`.
4. The answer will be the maximum value in the last row of `dp`.

**Time Complexity**: O(N^3) where N is the number of rows (assuming square matrix).

**Space Complexity**: O(N^2)

```typescript
function maxPoints(points: number[][]): number {
    const m = points.length;
    const n = points[0].length;
    const dp: number[][] = Array.from({ length: m }, () => Array(n).fill(0));

    // Initialize the first row of dp with points[0]
    for (let j = 0; j < n; j++) {
        dp[0][j] = points[0][j];
    }

    for (let i = 1; i < m; i++) {
        for (let j = 0; j < n; j++) { // Current column in row i
            for (let k = 0; k < n; k++) { // Previous row
                // Check each k in the previous row
                dp[i][j] = Math.max(dp[i][j], dp[i-1][k] + points[i][j] - Math.abs(k - j));
            }
        }
    }

    // Find the maximum in the last row of dp
    return Math.max(...dp[m - 1]);
}
```

### Approach 2: Dynamic Programming with Pre-calculation O(N^2) Complexity

**Intuition**:

The O(N^3) complexity can be improved by optimizing the calculation of maximum possible values from the previous row by using two arrays `left` and `right`, which pre-calculate the maximum points attainable if moving left-to-right and right-to-left respectively. `left[i][j]` captures maximum points attainable to column `j` while moving left-to-right, and `right[i][j]` captures them moving right-to-left.

**Approach**:

1. For each row `i`, compute a `left` array where `left[j]` is the best value to achieve column `j` when moving from left.
2. Similarly, compute a `right` array for best values when moving from the right.
3. Calculate `dp[i][j]` by considering `left[j]` and `right[j]` to optimize selection of `j` from `i-1` row.
   
**Time Complexity**: O(N^2) — Calculating left and right arrays for each row takes linear time.

**Space Complexity**: O(N) for storing left and right arrays.

```typescript
function maxPointsOptimized(points: number[][]): number {
    const m = points.length;
    const n = points[0].length;
    let prevRow = points[0];

    for (let i = 1; i < m; i++) {
        const left = Array(n).fill(0);
        const right = Array(n).fill(0);

        // Populate the left array
        left[0] = prevRow[0];
        for (let j = 1; j < n; j++) {
            left[j] = Math.max(left[j - 1] - 1, prevRow[j]);
        }

        // Populate the right array
        right[n - 1] = prevRow[n - 1];
        for (let j = n - 2; j >= 0; j--) {
            right[j] = Math.max(right[j + 1] - 1, prevRow[j]);
        }

        // Calculate the current row dp values
        const currentRow = Array(n).fill(0);
        for (let j = 0; j < n; j++) {
            currentRow[j] = points[i][j] + Math.max(left[j], right[j]);
        }
        
        // Move to the next row
        prevRow = currentRow;
    }

    // The answer is the maximum value in the last processed row
    return Math.max(...prevRow);
}
```

This approach significantly reduces the time complexity, allowing it to handle larger input sizes efficiently while maintaining an easy-to-understand transition mechanism by using left-right pre-processing!

