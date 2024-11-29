# [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approach 1: Simulation with Boundaries (Optimal)

### Solution
```python
# Time Complexity: O(m * n)
# Space Complexity: O(1) (excluding output list)

from typing import List

class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        result = []

        if not matrix or not matrix[0]:
            return result

        top, bottom = 0, len(matrix) - 1
        left, right = 0, len(matrix[0]) - 1

        while top <= bottom and left <= right:
            # Traverse from left to right
            for i in range(left, right + 1):
                result.append(matrix[top][i])
            top += 1

            # Traverse from top to bottom
            for i in range(top, bottom + 1):
                result.append(matrix[i][right])
            right -= 1

            # Traverse from right to left
            if top <= bottom:
                for i in range(right, left - 1, -1):
                    result.append(matrix[bottom][i])
                bottom -= 1

            # Traverse from bottom to top
            if left <= right:
                for i in range(bottom, top - 1, -1):
                    result.append(matrix[i][left])
                left += 1

        return result
```

## Approach 2: Simulated Layer Traversal (Alternative)

### Solution
```python
# Time Complexity: O(m * n)
# Space Complexity: O(1) (excluding output list)

from typing import List

class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        result = []

        if not matrix or not matrix[0]:
            return result

        rows, cols = len(matrix), len(matrix[0])
        startRow, startCol = 0, 0

        while startRow < rows and startCol < cols:
            # Traverse the top row
            for i in range(startCol, cols):
                result.append(matrix[startRow][i])
            startRow += 1

            # Traverse the right column
            for i in range(startRow, rows):
                result.append(matrix[i][cols - 1])
            cols -= 1

            # Traverse the bottom row
            if startRow < rows:
                for i in range(cols - 1, startCol - 1, -1):
                    result.append(matrix[rows - 1][i])
                rows -= 1

            # Traverse the left column
            if startCol < cols:
                for i in range(rows - 1, startRow - 1, -1):
                    result.append(matrix[i][startCol])
                startCol += 1

        return result
```

