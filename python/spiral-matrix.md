## [Leetcode Problem 54: Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

### Approaches
- [Approach 1: Simulation using Boundaries](#approach-1-simulation-using-boundaries)

---

### Approach 1: Simulation using Boundaries

**Intuition:**

The problem requires traversing a matrix in a spiral order. Intuitively, to achieve this task, we can simulate the process by defining boundaries. Start from the top-left corner of the matrix, move towards the right, then traverse downward, move leftwards and finally go upwards, reducing the traversal boundaries each time a direction is completed. Repeat this process until all elements are traversed.

**Approach:**

1. Define four boundaries: `top`, `bottom`, `left`, and `right`.
2. Initiate these boundaries with:
   - `top = 0` and `bottom = rows - 1`
   - `left = 0` and `right = columns - 1`
3. Traverse the matrix in a spiral order using the boundaries:
    - Traverse from `left` to `right` using `top` and then increment the `top` boundary.
    - Traverse from `top` to `bottom` using `right` and then decrement the `right` boundary.
    - Traverse from `right` to `left` using `bottom` and then decrement the `bottom` boundary, only if `top <= bottom`.
    - Traverse from `bottom` to `top` using `left` and then increment the `left` boundary, only if `left <= right`.
4. Continue the process while `top <= bottom` and `left <= right`.
  
**Time Complexity:** `O(M * N)`, where `M` is the number of rows and `N` is the number of columns since each element is visited exactly once.
   
**Space Complexity:** `O(1)` if we exclude the space required for the output array; otherwise, `O(M * N)` due to storage of the result.

Here's the Python implementation:

```python
def spiralOrder(matrix):
    # Define the boundaries
    result = []
    if not matrix:
        return result

    # Initialize the traversal boundaries
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1

    while top <= bottom and left <= right:
        # Traverse from left to right across the top row
        for i in range(left, right + 1):
            result.append(matrix[top][i])
        top += 1  # Move the top boundary down
        
        # Traverse from top to bottom down the right column
        for i in range(top, bottom + 1):
            result.append(matrix[i][right])
        right -= 1  # Move the right boundary left
        
        # Traverse from right to left across the bottom row
        if top <= bottom:
            for i in range(right, left - 1, -1):
                result.append(matrix[bottom][i])
            bottom -= 1  # Move the bottom boundary up
        
        # Traverse from bottom to top up the left column
        if left <= right:
            for i in range(bottom, top - 1, -1):
                result.append(matrix[i][left])
            left += 1  # Move the left boundary right

    return result
```

---

This approach efficiently simulates the spiral order traversal by adjusting the boundaries and ensures we cover all matrix elements without missing or revisiting any.

