# [Leetcode Problem 48: Rotate Image](https://leetcode.com/problems/rotate-image/)

## Approaches
1. [Approach 1: Intermediate Matrix Rotation](#approach-1-intermediate-matrix-rotation)
2. [Approach 2: In-Place Rotation with Matrix Transpose and Reverse](#approach-2-in-place-rotation-with-matrix-transpose-and-reverse)

---

## Approach 1: Intermediate Matrix Rotation

Intuition:
- The problem requires rotating the matrix by 90 degrees clockwise.
- One approach is to use an intermediate matrix to perform the rotation.
- The position (i, j) in the original matrix becomes (j, n-1-i) in the rotated matrix.
  
This solution is straightforward but requires additional space to store the intermediate result.

### Code

```cpp
#include <vector>
using namespace std;

void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    vector<vector<int>> rotated(n, vector<int>(n)); // Create an intermediate matrix
    
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            // Set the rotated matrix's element based on the calculations
            rotated[j][n - 1 - i] = matrix[i][j];
        }
    }

    // Copy rotated matrix back to original matrix
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            matrix[i][j] = rotated[i][j];
        }
    }
}
```

### Complexity Analysis

- **Time Complexity**: O(n^2), as we are iterating through each element of the matrix.
- **Space Complexity**: O(n^2), for the intermediate matrix used to store the result.

---

## Approach 2: In-Place Rotation with Matrix Transpose and Reverse

Intuition:
- To optimize the space complexity, we can perform the rotation in-place.
- This can be achieved by two main operations:
  1. Transpose the matrix.
  2. Reverse each row of the transposed matrix.
- Transposing switches rows with columns, and reversing each row achieves the effect of rotating by 90 degrees clockwise.

### Code

```cpp
#include <vector>
using namespace std;

void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    
    // Transpose the matrix
    for (int i = 0; i < n; ++i) {
        for (int j = i; j < n; ++j) {
            // Swap element (i, j) with element (j, i)
            swap(matrix[i][j], matrix[j][i]);
        }
    }
    
    // Reverse each row
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n / 2; ++j) {
            // Swap elements to reverse the row
            swap(matrix[i][j], matrix[i][n - 1 - j]);
        }
    }
}
```

### Complexity Analysis

- **Time Complexity**: O(n^2), as we are iterating through each element of a matrix twice—once for transposing and once for reversing.
- **Space Complexity**: O(1), since the rotation is done in-place without using any additional space.

