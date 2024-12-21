# [Leetcode 1277: Count Square Submatrices with All Ones](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

---

## Approach 1: Brute Force

### Intuition

The brute-force approach involves checking every possible submatrix in the grid, counting how many such submatrices are entirely filled with 1's. For each starting point in the matrix, attempt to create the largest possible square submatrix, verifying each square as you increase its size. This ensures that you account for all squares starting from that point.

### Time Complexity

The time complexity here is \(O(n^3)\) where \(n\) is the dimension of the grid. This is because for each cell, we might check up to n submatrices starting at that point.

### Space Complexity

The space complexity is \(O(1)\) since we only use a few variables for counting and iterating, with no additional data structures of size relative to the input.

### Python Code

```python
def countSquares(matrix):
    if not matrix or not matrix[0]:
        return 0

    rows, cols = len(matrix), len(matrix[0])
    total_squares = 0

    # Iterate over every possible top-left corner of a square
    for r in range(rows):
        for c in range(cols):
            # Trying different sizes of squares
            for size in range(min(rows - r, cols - c)):
                if all(matrix[r + i][c + j] == 1 for i in range(size + 1) for j in range(size + 1)):
                    total_squares += 1
                else:
                    break  # Once we find a non-1, we cannot form a larger square

    return total_squares
```

---

## Approach 2: Dynamic Programming

### Intuition

The idea here is to use dynamic programming to store the size of the largest square submatrix ending at each cell (i, j). 

- If the element is `1`, check the minimum of its top, left, and top-left neighbors (since they can help form a square) and add one (for the current square of size 1). 
- This value represents the maximum size of a square that can be formed with the bottom-right corner at this element.

### Time Complexity

The time complexity is \(O(n \times m)\), where \(n\) is the number of rows and \(m\) is the number of columns since we traverse each element once.

### Space Complexity

The space complexity is \(O(n \times m)\) as we store a value for each state in the dp matrix. However, it can be reduced to \(O(m)\) if you modify the input matrix in place or use only one-dimensional arrays.

### Python Code

```python
def countSquares(matrix):
    if not matrix or not matrix[0]:
        return 0

    rows, cols = len(matrix), len(matrix[0])
    dp = [[0] * cols for _ in range(rows)]
    total_squares = 0

    for r in range(rows):
        for c in range(cols):
            # If it's a 1, we can potentially form squares
            if matrix[r][c] == 1:
                if r == 0 or c == 0:
                    # Base case where any 1 on the first row or first column can only form a 1x1 square
                    dp[r][c] = 1
                else:
                    # Find the minimum square we can form
                    dp[r][c] = min(dp[r-1][c], dp[r][c-1], dp[r-1][c-1]) + 1

                # Accumulate the count of squares
                total_squares += dp[r][c]

    return total_squares
```

Both approaches solve the problem, but the dynamic programming approach is significantly more optimized for larger inputs.

