# [Leetcode 73: Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

## Approaches
1. [Brute Force](#approach-1-brute-force)
2. [Extra Space for Zero Tracking](#approach-2-extra-space-for-zero-tracking)
3. [In-Place Space Optimization (Using First Row/Column as Marker)](#approach-3-in-place-space-optimization-using-first-rowcolumn-as-marker)

### Approach 1: Brute Force

#### Intuition
In this naive approach, whenever we encounter a zero in the matrix, we immediately set all elements in its row and column to zero. This approach is inefficient because the zero introduced in a different row/column might again trigger setting all elements of another row/column to zero.

#### Steps
1. Traverse the entire matrix to find all zero elements.
2. For each zero found, iterate over its corresponding row and column and set all elements in them to zero.
3. This requires checking for zero for all matrix elements repeatedly, leading to inefficiencies.

#### Code
```python
def setZeroes(matrix):
    rows, cols = len(matrix), len(matrix[0])
    zeroes = [(i, j) for i in range(rows) for j in range(cols) if matrix[i][j] == 0]

    for (i, j) in zeroes:
        for x in range(cols):
            matrix[i][x] = 0  # Set the entire row to zero
        for y in range(rows):
            matrix[y][j] = 0  # Set the entire column to zero
```

#### Time Complexity
- **O((M * N) * (M + N))**: The loops traverse each element once plus additional work for each zero. M is number of rows, N is number of columns.

#### Space Complexity
- **O(M * N)**: Storing the zeroes positions could take up to this much space in the worst case.

### Approach 2: Extra Space for Zero Tracking

#### Intuition
Instead of modifying rows and columns immediately upon discovering a zero, we can record the rows and columns that need to be zeroed in separate sets. After identifying all such rows and columns, we zero them in a second pass.

#### Steps
1. Traverse the matrix to find and record the indices of rows and columns containing zeroes.
2. In a second pass, zero out the rows and columns that are recorded.

#### Code
```python
def setZeroes(matrix):
    rows, cols = len(matrix), len(matrix[0])
    zero_rows, zero_cols = set(), set()

    # Record all zero positions
    for i in range(rows):
        for j in range(cols):
            if matrix[i][j] == 0:
                zero_rows.add(i)
                zero_cols.add(j)

    # Set rows to zero
    for i in zero_rows:
        for j in range(cols):
            matrix[i][j] = 0

    # Set columns to zero
    for j in zero_cols:
        for i in range(rows):
            matrix[i][j] = 0
```

#### Time Complexity
- **O(M * N)**: Two separate passes through the matrix are required, one to find zeros and one to zero out.

#### Space Complexity
- **O(M + N)**: Space required to store unique row and column indices.

### Approach 3: In-Place Space Optimization (Using First Row/Column as Marker)

#### Intuition
An optimal in-place solution involves using the first row and first column as markers to indicate which row or column should be zeroed. However, altering the first row/column as markers can destructively interfere with the original row or column values. Thus, we also maintain a separate flag for determining whether the first row or the first column should be zeroed.

#### Steps
1. Determine if the first row and the first column should be zeroed, by checking their initial values.
2. Use the first row and column as markers to note which rows and columns should be zeroed for the rest of the matrix.
3. Set the matrix elements to zero accordingly using the markers.
4. Finally, zero out the first row and column if needed, based on the initial checks.

#### Code
```python
def setZeroes(matrix):
    rows, cols = len(matrix), len(matrix[0])
    first_row_zero = any(matrix[0][j] == 0 for j in range(cols))
    first_col_zero = any(matrix[i][0] == 0 for i in range(rows))

    # Mark zeros in the first row/column
    for i in range(1, rows):
        for j in range(1, cols):
            if matrix[i][j] == 0:
                matrix[i][0] = 0
                matrix[0][j] = 0

    # Zero out cells based on markers in the first row/column
    for i in range(1, rows):
        for j in range(1, cols):
            if matrix[i][0] == 0 or matrix[0][j] == 0:
                matrix[i][j] = 0

    # Zero out the first column if necessary
    if first_col_zero:
        for i in range(rows):
            matrix[i][0] = 0

    # Zero out the first row if necessary
    if first_row_zero:
        for j in range(cols):
            matrix[0][j] = 0
```

#### Time Complexity
- **O(M * N)**: The matrix is traversed a few times in a controlled manner.

#### Space Complexity
- **O(1)**: Uses minimal extra space beyond the modifications to the input itself. 

This approach effectively reduces the space complexity while maintaining readability and clarity of operations on the matrix.

