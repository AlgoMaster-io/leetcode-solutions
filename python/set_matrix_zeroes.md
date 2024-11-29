# [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

## Approach 1: Using Additional Arrays (Basic)

### Solution
python
```python
# Time Complexity: O(m * n)
# Space Complexity: O(m + n)
class Solution:
    def setZeroes(self, matrix):
        rows = len(matrix)
        cols = len(matrix[0])
        
        rowZero = [False] * rows
        colZero = [False] * cols
        
        # Record rows and columns that need to be zeroed
        for i in range(rows):
            for j in range(cols):
                if matrix[i][j] == 0:
                    rowZero[i] = True
                    colZero[j] = True
        
        # Set rows to zero
        for i in range(rows):
            if rowZero[i]:
                for j in range(cols):
                    matrix[i][j] = 0

        # Set columns to zero
        for j in range(cols):
            if colZero[j]:
                for i in range(rows):
                    matrix[i][j] = 0
```

## Approach 2: Using First Row and Column as Markers (Optimal)

### Solution
python
```python
# Time Complexity: O(m * n)
# Space Complexity: O(1)
class Solution:
    def setZeroes(self, matrix):
        rows = len(matrix)
        cols = len(matrix[0])
        
        firstRowZero = False
        firstColZero = False
        
        # Check if first row needs to be zeroed
        for j in range(cols):
            if matrix[0][j] == 0:
                firstRowZero = True
                break

        # Check if first column needs to be zeroed
        for i in range(rows):
            if matrix[i][0] == 0:
                firstColZero = True
                break
        
        # Use first row and column as markers
        for i in range(1, rows):
            for j in range(1, cols):
                if matrix[i][j] == 0:
                    matrix[i][0] = 0
                    matrix[0][j] = 0
        
        # Set elements to zero based on markers
        for i in range(1, rows):
            for j in range(1, cols):
                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0
        
        # Zero out the first row if needed
        if firstRowZero:
            for j in range(cols):
                matrix[0][j] = 0
        
        # Zero out the first column if needed
        if firstColZero:
            for i in range(rows):
                matrix[i][0] = 0
```

