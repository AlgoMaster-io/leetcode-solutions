# LeetCode Problem: [48. Rotate Image](https://leetcode.com/problems/rotate-image/)

## Table of Contents
- [Approach 1: Transposing and Reversing Each Row](#approach-1-transposing-and-reversing-each-row)
- [Approach 2: Cyclic Swaps](#approach-2-cyclic-swaps)

---

## Approach 1: Transposing and Reversing Each Row

### Intuition
The rotation of the matrix by 90 degrees can be achieved in two main steps:
1. **Transpose the matrix**: This involves converting each element at position `(i, j)` to position `(j, i)`. This will make rows become columns.
2. **Reverse each row**: After transposing, reversing each row will simulate a 90-degree rotation as expected.

This method leverages the properties of matrix transpose and reversal to arrive at the desired rotation.

### C# Code Implementation
```csharp
public class Solution {
    public void Rotate(int[][] matrix) {
        int n = matrix.Length;

        // Transpose the matrix
        for (int i = 0; i < n; ++i) {
            for (int j = i; j < n; ++j) {
                // Swap element at (i, j) with element at (j, i)
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Reverse each row
        for (int i = 0; i < n; ++i) {
            // Use two pointers to reverse the row
            int left = 0, right = n - 1;
            while (left < right) {
                // Swap elements to reverse the row
                int temp = matrix[i][left];
                matrix[i][left] = matrix[i][right];
                matrix[i][right] = temp;
                left++;
                right--;
            }
        }
    }
}
```

### Time Complexity
- **O(n^2)**: We traverse the matrix twice, once for transposing and once for reversing each row.
  
### Space Complexity
- **O(1)**: The manipulation is done in-place, using constant extra space.

---

## Approach 2: Cyclic Swaps

### Intuition
Instead of a two-step process, we can rotate the matrix by directly performing swaps in a four-way cycle. Starting from the outer layer and moving towards the inner layers, the elements are moved such that four positions are rotated at once.

### Steps
- Divide the matrix into layers, starting from the outermost layer towards the innermost.
- For each layer, perform a series of swaps to rotate the corners and move the edges appropriately.

### C# Code Implementation
```csharp
public class Solution {
    public void Rotate(int[][] matrix) {
        int n = matrix.Length;
        
        // Iterate over each layer from outermost to the central layer
        for (int layer = 0; layer < n / 2; layer++) {
            int first = layer; // Starting index of the layer
            int last = n - 1 - layer; // Ending index of the layer

            for (int i = first; i < last; i++) {
                int offset = i - first;
                // Save the top element
                int top = matrix[first][i];
                
                // left -> top
                matrix[first][i] = matrix[last - offset][first];
                
                // bottom -> left
                matrix[last - offset][first] = matrix[last][last - offset];
                
                // right -> bottom
                matrix[last][last - offset] = matrix[i][last];
                
                // top -> right
                matrix[i][last] = top; // Assign saved top element to right
            }
        }
    }
}
```

### Time Complexity
- **O(n^2)**: We perform the swaps for each element only once, so it results in traversing through all elements of the matrix.
  
### Space Complexity
- **O(1)**: The algorithm utilizes in-place swaps, requiring no additional space for another matrix.

---

These approaches provide clear methods to rotate a matrix, each utilizing distinct methods to achieve the transformation effectively.

