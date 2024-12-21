# [Leetcode 1937: Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming with Optimized Space](#dynamic-programming-with-optimized-space)

### Brute Force Approach

#### Intuition
The problem statement requires calculating the maximum number of points that can be obtained while considering the costs associated with moving between columns for multiple rows. The initial brute force thought involves trying each cell in the first row and moving row by row attempting to select the optimal cells to maximise the score. This approach can be extremely slow and inefficient as it explores all possible paths.

#### Steps
1. Iterate over each cell in the first row.
2. For each starting cell, apply a recursive function to evaluate the possible scores by moving to all valid cells of the next row.
3. Sum the maximum possibilities by traversing through the matrix till the last row.
4. Return the maximum score that is achievable by trying all starting positions in the first row.

#### Code
```javascript
function maxPoints(points) {
    function helper(row, col) {
        if (row >= points.length) return 0;
        let maxScore = 0;
        
        for (let nextCol = 0; nextCol < points[0].length; nextCol++) {
            const moveCost = Math.abs(col - nextCol);
            const score = points[row][col] - moveCost + helper(row + 1, nextCol);
            maxScore = Math.max(maxScore, score);
        }
        return maxScore;
    }
    
    let maxPoints = 0;
    for (let col = 0; col < points[0].length; col++) {
        // Consider starting from each column in the first row
        maxPoints = Math.max(maxPoints, helper(0, col));
    }
    return maxPoints;
}
```

#### Complexity
- Time Complexity: \(O(M \times M \times N)\) where \(M\) is the number of columns, and \(N\) is the number of rows.
- Space Complexity: \(O(M \times N)\) due to the recursion call stack.

### Dynamic Programming with Optimized Space

#### Intuition
The brute force approach is inefficient due to repeated calculations for each possible path. By using dynamic programming, we aim to store results of subproblems (i.e., each row's results for each column) so that they don't need to be recalculated multiple times. The concept involves maintaining two auxiliary arrays to store the best scores one can achieve moving left-to-right and right-to-left for each row, enabling us to efficiently calculate possible scores for subsequent rows.

#### Steps
1. Start by initializing a dp array with values of the first row.
2. For each subsequent row, prepare two auxiliary arrays: `left` and `right`. 
    - `left[i]` stores the maximum score available by considering moving from left up to `i`.
    - `right[i]` does a similar calculation but moves from right to left.
3. Combine results to compute the dp array for current row taking previous row into account with the above derived arrays.
4. After processing all rows, the maximum value in the last computed dp array will be the final result.

#### Code
```javascript
function maxPoints(points) {
    const m = points.length;
    const n = points[0].length;
    
    let dp = points[0];
    
    for (let i = 1; i < m; i++) {
        let left = new Array(n).fill(0);
        let right = new Array(n).fill(0);
        
        left[0] = dp[0];
        for (let j = 1; j < n; j++) {
            left[j] = Math.max(left[j - 1] - 1, dp[j]);
        }
        
        right[n - 1] = dp[n - 1];
        for (let j = n - 2; j >= 0; j--) {
            right[j] = Math.max(right[j + 1] - 1, dp[j]);
        }
        
        for (let j = 0; j < n; j++) {
            dp[j] = points[i][j] + Math.max(left[j], right[j]);
        }
    }
    
    return Math.max(...dp);
}
```

#### Complexity
- Time Complexity: \(O(N \times M)\) where \(N\) is the number of rows and \(M\) is the number of columns.
- Space Complexity: \(O(M)\) given that only two additional arrays proportional to the number of columns are needed.

