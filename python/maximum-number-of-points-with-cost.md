## [Leetcode 1937: Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming with Prefix Max Arrays](#dynamic-programming-with-prefix-max-arrays)
3. [Optimized Dynamic Programming](#optimized-dynamic-programming)

### Brute Force Approach

**Intuition:**

In this approach, for every row, we calculate the cost of coming from each column of the previous row to every column in the current row. This involves iterating over pairs of columns between two consecutive rows to calculate all possible costs.

**Code:**
```python
def maxPoints(points):
    rows, cols = len(points), len(points[0])
    dp = [0] * cols

    for r in range(rows):
        new_dp = [0] * cols
        for c1 in range(cols):
            max_cost = 0
            for c2 in range(cols):
                # Calculate the cost of moving from column c2 in the previous row
                # to column c1 in the current row, and keep track of the maximum.
                cost = dp[c2] - abs(c1 - c2)
                if cost > max_cost:
                    max_cost = cost
            # Add the points of the current cell.
            new_dp[c1] = max_cost + points[r][c1]
        dp = new_dp
    
    # Return the maximum number of points at the last row.
    return max(dp)
```

**Time Complexity:** O(n * m^2), where `n` is the number of rows and `m` is the number of columns.
**Space Complexity:** O(m), the space used for keeping the dynamic programming table.

### Dynamic Programming with Prefix Max Arrays

**Intuition:**

Instead of evaluating all possible pairs of column transitions, we use two auxiliary arrays, `left` and `right`, to maintain the maximum points considering transition costs from left and right directions. This enables efficient computation of the maximum points for each position in a row.

**Code:**
```python
def maxPoints(points):
    rows, cols = len(points), len(points[0])
    dp = points[0]
    
    for r in range(1, rows):
        left = [0] * cols
        right = [0] * cols
        new_dp = [0] * cols
        
        # Calculate left max array, taking the transitions from left to right
        left[0] = dp[0]
        for c in range(1, cols):
            left[c] = max(left[c-1] - 1, dp[c])
        
        # Calculate right max array, taking the transitions from right to left
        right[-1] = dp[-1]
        for c in range(cols-2, -1, -1):
            right[c] = max(right[c+1] - 1, dp[c])
        
        # Calculate the maximum number of points for the current row
        for c in range(cols):
            new_dp[c] = points[r][c] + max(left[c], right[c])
        
        dp = new_dp
    
    return max(dp)
```

**Time Complexity:** O(n * m), where `n` is the number of rows and `m` is the number of columns.
**Space Complexity:** O(m), for storing the arrays `left`, `right`, and the current `dp`.

### Optimized Dynamic Programming

**Intuition:**

Further refining the approach, we integrate the calculation of transition costs directly into a single loop. This method combines the logic of maintaining left and right max into a single pass over the columns, reducing the space used by auxiliary arrays.

**Code:**
```python
def maxPoints(points):
    rows, cols = len(points), len(points[0])
    dp = points[0]

    for r in range(1, rows):
        new_dp = [0] * cols
        curr_max = dp[0]
        
        # Left to right pass for current row max calculation
        for c in range(cols):
            curr_max = max(curr_max - 1, dp[c])
            new_dp[c] = curr_max
        
        # Right to left pass to finalize the maximum with the current row's points
        curr_max = dp[-1]
        for c in range(cols-1, -1, -1):
            curr_max = max(curr_max - 1, dp[c])
            new_dp[c] = max(new_dp[c], curr_max + points[r][c])
        
        dp = new_dp

    return max(dp)
```

**Time Complexity:** O(n * m), where `n` is the number of rows and `m` is the number of columns.
**Space Complexity:** O(m), reduced by using only a single `dp` array.

