## [Leetcode 73: Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Additional Arrays](#approach-2-using-additional-arrays)
- [Approach 3: Optimal In-Place Soluton](#approach-3-optimal-in-place-solution)

### Approach 1: Brute Force

**Intuition:**

The brute force method involves identifying which rows and columns should be set to zero by iterating over the matrix. We first create a copy of the matrix. Using this copy, we iterate over the entire original matrix. If an element is zero, we set all the elements in the corresponding row and column in the copied matrix to zero. After processing, the original matrix is updated to match the copied matrix.

**Steps:**
1. Create a copy of the original matrix.
2. Iterate over each element. If an element is 0, mark the entire row and column in the copied matrix as 0.
3. Finally, copy the values from the new matrix to the original matrix.

**Code:**

```java
public void setZeroes(int[][] matrix) {
    int rows = matrix.length;
    int cols = matrix[0].length;
    int[][] copy = new int[rows][cols];
    
    // Copy the original matrix
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            copy[r][c] = matrix[r][c];
        }
    }
    
    // Process the matrix using copy
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            if (matrix[r][c] == 0) {
                // Set entire row to zero
                for (int rc = 0; rc < cols; rc++) {
                    copy[r][rc] = 0;
                }
                // Set entire column to zero
                for (int rr = 0; rr < rows; rr++) {
                    copy[rr][c] = 0;
                }
            }
        }
    }
    
    // Copy back the zeroed matrix
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            matrix[r][c] = copy[r][c];
        }
    }
}
```

**Complexity:**

- Time Complexity: \(O(m \cdot n)\), where \(m\) is the number of rows and \(n\) is the number of columns.
- Space Complexity: \(O(m \cdot n)\) due to the additional matrix copy.

### Approach 2: Using Additional Arrays

**Intuition:**

Instead of using a complete matrix to keep track of which rows and columns need to be zeroed, we can use two separate arrays. One for rows and another for columns that need to be zeroed. This makes the algorithm more space efficient.

**Steps:**
1. Use two arrays `zeroRows` and `zeroCols` to track rows and columns with zeroes.
2. First, iterate through the original matrix to fill `zeroRows` and `zeroCols`.
3. Iterate again through the matrix, using `zeroRows` and `zeroCols` to set the appropriate rows and columns to zero.

**Code:**

```java
public void setZeroes(int[][] matrix) {
    int rows = matrix.length;
    int cols = matrix[0].length;
    boolean[] zeroRows = new boolean[rows];
    boolean[] zeroCols = new boolean[cols];

    // First pass to record zero rows and columns
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            if (matrix[r][c] == 0) {
                zeroRows[r] = true;
                zeroCols[c] = true;
            }
        }
    }

    // Second pass to set zero rows and columns
    for (int r = 0; r < rows; r++) {
        for (int c = 0; c < cols; c++) {
            if (zeroRows[r] || zeroCols[c]) {
                matrix[r][c] = 0;
            }
        }
    }
}
```

**Complexity:**

- Time Complexity: \(O(m \cdot n)\)
- Space Complexity: \(O(m + n)\) for the row and column arrays.

### Approach 3: Optimal In-Place Solution

**Intuition:**

To achieve O(1) additional space complexity, we can use the input matrix itself to store the required information. We mark the first cell of the respective row and column as zero if we encounter a zero during our matrix traversal. We should be careful while marking to save the state of the first row and column initially because they will be modified during the marking process.

**Steps:**
1. Determine if the first row and first column need to be zeroed for later reference.
2. Use the first row and first column to mark whether a row or column should be zero.
3. Iterate over the matrix starting from `[1][1]`, and use the markers in the first row and column to set zeroes.
4. Finally, handle zeroing of the first row and column based on the stored state in step 1.

**Code:**

```java
public void setZeroes(int[][] matrix) {
    int rows = matrix.length;
    int cols = matrix[0].length;

    // Determine if first row and column will need zeroing
    boolean firstRowZero = false, firstColZero = false;

    for (int c = 0; c < cols; c++) {
        if (matrix[0][c] == 0) {
            firstRowZero = true;
            break;
        }
    }

    for (int r = 0; r < rows; r++) {
        if (matrix[r][0] == 0) {
            firstColZero = true;
            break;
        }
    }

    // Use first row and column as markers
    for (int r = 1; r < rows; r++) {
        for (int c = 1; c < cols; c++) {
            if (matrix[r][c] == 0) {
                matrix[r][0] = 0;
                matrix[0][c] = 0;
            }
        }
    }

    // Set matrix zeroes using markers
    for (int r = 1; r < rows; r++) {
        for (int c = 1; c < cols; c++) {
            if (matrix[r][0] == 0 || matrix[0][c] == 0) {
                matrix[r][c] = 0;
            }
        }
    }

    // Zero first row if needed
    if (firstRowZero) {
        for (int c = 0; c < cols; c++) {
            matrix[0][c] = 0;
        }
    }

    // Zero first column if needed
    if (firstColZero) {
        for (int r = 0; r < rows; r++) {
            matrix[r][0] = 0;
        }
    }
}
```

**Complexity:**

- Time Complexity: \(O(m \cdot n)\)
- Space Complexity: \(O(1)\), utilizing the matrix itself for storage.

