# [Leetcode 54: Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approaches:
1. [Simulated Spiral Traversal](#simulated-spiral-traversal)

### Simulated Spiral Traversal

#### Intuition
To solve the problem of traversing a matrix in spiral order, the most intuitive approach is to simulate the spiral traversal by using boundaries. We maintain four boundaries (top, bottom, left, right) which define the current bounds of the spiral and adjust them as we traverse the matrix in layers until the traversal is completed.

#### Approach
1. Initialize variables `top`, `bottom`, `left`, and `right` to represent the current boundaries of the matrix traversal.
2. Create a list to store the result of the spiral order.
3. While the top boundary is less than or equal to the bottom boundary and the left boundary is less than or equal to the right boundary, perform the following steps:
   - Traverse from the left boundary to the right boundary along the top row. Increment the `top` boundary after this.
   - Traverse downwards from the top boundary to the bottom boundary along the right column. Decrement the `right` boundary after this.
   - If the top boundary is still less than or equal to the bottom boundary, traverse from the right boundary to the left boundary along the bottom row. Decrement the `bottom` boundary after this.
   - If the left boundary is still less than or equal to the right boundary, traverse upwards from the bottom boundary to the top boundary along the left column. Increment the `left` boundary after this.
4. After the loop ends, the result list will contain the matrix elements in spiral order.

#### Time Complexity
- O(m * n), where m is the number of rows and n is the number of columns in the matrix. We visit each element exactly once.

#### Space Complexity
- O(1), not counting the output list as we are using only a fixed number of extra variables regardless of the input size.

```csharp
public class Solution {
    public IList<int> SpiralOrder(int[][] matrix) {
        var result = new List<int>();
        
        if (matrix == null || matrix.Length == 0) return result;
        
        int top = 0, bottom = matrix.Length - 1;
        int left = 0, right = matrix[0].Length - 1;
        
        // Continue the spiral traversal until the boundaries cross each other
        while (top <= bottom && left <= right) {
            // Traverse from left to right on the current top row
            for (int col = left; col <= right; col++) {
                result.Add(matrix[top][col]);
            }
            top++;
            
            // Traverse from top to bottom on the current right column
            for (int row = top; row <= bottom; row++) {
                result.Add(matrix[row][right]);
            }
            right--;
            
            // Condition to check if there are remaining rows
            if (top <= bottom) {
                // Traverse from right to left on the current bottom row
                for (int col = right; col >= left; col--) {
                    result.Add(matrix[bottom][col]);
                }
                bottom--;
            }
            
            // Condition to check if there are remaining columns
            if (left <= right) {
                // Traverse from bottom to top on the current left column
                for (int row = bottom; row >= top; row--) {
                    result.Add(matrix[row][left]);
                }
                left++;
            }
        }
        
        return result;
    }
}
```

This approach efficiently accumulates the matrix's elements into a spiral order by progressively narrowing down the traversal boundaries until all elements are collected. The spiral traversal is achieved iteratively for each layer defined by the boundaries.

