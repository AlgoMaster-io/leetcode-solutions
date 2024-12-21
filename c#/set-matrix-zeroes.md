# [Leetcode 73: Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Additional Memory](#approach-2-using-additional-memory)
- [Approach 3: Optimal In-Place Solution](#approach-3-optimal-in-place-solution)

### Approach 1: Brute Force

#### Intuition
In this approach, we make a copy of the original matrix. We iterate over each element, and whenever we encounter a zero, we set the entire row and column of that position to zero in the copied matrix. Finally, we copy the modified matrix back to the original matrix.

#### Steps
1. Create a copy of the matrix.
2. Traverse each element in the matrix.
3. If an element is zero, set its entire row and column in the copy to zero.
4. Update the original matrix with values from the copied matrix.

#### Code
```csharp
public class Solution {
    public void SetZeroes(int[][] matrix) {
        int rows = matrix.Length;
        int cols = matrix[0].Length;
        int[][] copy = new int[rows][];
        
        // Copy the original matrix to the new matrix
        for(int i = 0; i < rows; i++) {
            copy[i] = new int[cols];
            for (int j = 0; j < cols; j++) {
                copy[i][j] = matrix[i][j];
            }
        }
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (matrix[i][j] == 0) {
                    // Set ith row and jth column to zero in the copied matrix
                    for (int k = 0; k < cols; k++)
                        copy[i][k] = 0;
                    for (int k = 0; k < rows; k++)
                        copy[k][j] = 0;
                }
            }
        }
        
        // Copy the modified matrix back to the original matrix
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                matrix[i][j] = copy[i][j];
            }
        }
    }
}
```

#### Complexity
- **Time Complexity**: O((M * N)^2) - Because for every zero element, we're traversing its entire row and column.
- **Space Complexity**: O(M * N) - We are making a copy of the matrix.

---

### Approach 2: Using Additional Memory

#### Intuition
Instead of creating a copy of the whole matrix, we can use two hash sets to keep track of rows and columns that need to be zeroed. This saves memory.

#### Steps
1. Create two sets to store the indices of rows and columns that have zeros.
2. Traverse the matrix and add the row and column indices of zero elements to the sets.
3. Iterate over the matrix again, and whenever the matrix has a row or column in the set, set that element to zero.

#### Code
```csharp
using System.Collections.Generic;

public class Solution {
    public void SetZeroes(int[][] matrix) {
        int rows = matrix.Length;
        int cols = matrix[0].Length;
        HashSet<int> zeroRows = new HashSet<int>();
        HashSet<int> zeroCols = new HashSet<int>();
        
        // First pass: identify the rows and columns to be zeroed
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (matrix[i][j] == 0) {
                    zeroRows.Add(i);
                    zeroCols.Add(j);
                }
            }
        }
        
        // Second pass: set matrix elements to zero if needed
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (zeroRows.Contains(i) || zeroCols.Contains(j)) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

#### Complexity
- **Time Complexity**: O(M * N) - Going through the matrix twice.
- **Space Complexity**: O(M + N) - Space for the hash sets to store the indices.

---

### Approach 3: Optimal In-Place Solution

#### Intuition
The most optimal way to implement this without any additional space is by using the matrix itself to mark rows and columns to be zeroed. The first row and first column of the matrix can be used to store this information since they will be zeroed anyway if a zero is found there.

#### Steps
1. Determine if the first row and column should be zeroed by checking for any zero in them initially.
2. Use the first row and column to mark other rows and columns that should be zeroed.
3. Traverse the rest of the matrix and zero out using the markers in the first row/column.
4. Zero out the first row and column as necessary.

#### Code
```csharp
public class Solution {
    public void SetZeroes(int[][] matrix) {
        int rows = matrix.Length;
        int cols = matrix[0].Length;
        
        bool firstRowZero = false;
        bool firstColZero = false;
        
        // Check if first row should be zero
        for (int j = 0; j < cols; j++) {
            if (matrix[0][j] == 0) {
                firstRowZero = true;
                break;
            }
        }
        
        // Check if first column should be zero
        for (int i = 0; i < rows; i++) {
            if (matrix[i][0] == 0) {
                firstColZero = true;
                break;
            }
        }
        
        // Use first row and column as markers
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        
        // Zero out cells based on markers
        for (int i = 1; i < rows; i++) {
            for (int j = 1; j < cols; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }
        
        // Zero out first row if needed
        if (firstRowZero) {
            for (int j = 0; j < cols; j++) {
                matrix[0][j] = 0;
            }
        }
        
        // Zero out first column if needed
        if (firstColZero) {
            for (int i = 0; i < rows; i++) {
                matrix[i][0] = 0;
            }
        }
    }
}
```

#### Complexity
- **Time Complexity**: O(M * N) - Each operation is linear with respect to the number of elements.
- **Space Complexity**: O(1) - No additional space apart from variables used.

This final approach is optimal in terms of both time and space complexity, and should be preferred for large matrices.

