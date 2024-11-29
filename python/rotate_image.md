# [48. Rotate Image](https://leetcode.com/problems/rotate-image/)

## Approach 1: Transpose and Reverse Rows (Optimal)

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def rotate(self, matrix):
        n = len(matrix)

        # Step 1: Transpose the matrix
        for i in range(n):
            for j in range(i, n):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]

        # Step 2: Reverse each row
        for i in range(n):
            for j in range(n // 2):
                matrix[i][j], matrix[i][n - j - 1] = matrix[i][n - j - 1], matrix[i][j]
```

## Approach 2: Rotating Layer by Layer (In-Place)

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def rotate(self, matrix):
        n = len(matrix)

        # Rotate each layer
        for layer in range(n // 2):
            first = layer
            last = n - layer - 1

            for i in range(first, last):
                offset = i - first

                # Save top element
                top = matrix[first][i]

                # Move left to top
                matrix[first][i] = matrix[last - offset][first]

                # Move bottom to left
                matrix[last - offset][first] = matrix[last][last - offset]

                # Move right to bottom
                matrix[last][last - offset] = matrix[i][last]

                # Move top to right
                matrix[i][last] = top
```


