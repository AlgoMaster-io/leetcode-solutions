# [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approach 1: Simulation with Boundaries (Optimal)

### Solution
```cpp
// Time Complexity: O(m * n)
// Space Complexity: O(1) (excluding output list)
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;

        if (matrix.empty() || matrix[0].empty()) return result;

        int top = 0, bottom = matrix.size() - 1;
        int left = 0, right = matrix[0].size() - 1;

        while (top <= bottom && left <= right) {
            // Traverse from left to right
            for (int i = left; i <= right; i++) {
                result.push_back(matrix[top][i]);
            }
            top++;

            // Traverse from top to bottom
            for (int i = top; i <= bottom; i++) {
                result.push_back(matrix[i][right]);
            }
            right--;

            // Traverse from right to left
            if (top <= bottom) {
                for (int i = right; i >= left; i--) {
                    result.push_back(matrix[bottom][i]);
                }
                bottom--;
            }

            // Traverse from bottom to top
            if (left <= right) {
                for (int i = bottom; i >= top; i--) {
                    result.push_back(matrix[i][left]);
                }
                left++;
            }
        }

        return result;
    }
};
```

## Approach 2: Simulated Layer Traversal (Alternative)

### Solution
```cpp
// Time Complexity: O(m * n)
// Space Complexity: O(1) (excluding output list)
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;

        if (matrix.empty() || matrix[0].empty()) return result;

        int rows = matrix.size(), cols = matrix[0].size();
        int startRow = 0, startCol = 0;

        while (startRow < rows && startCol < cols) {
            // Traverse the top row
            for (int i = startCol; i < cols; i++) {
                result.push_back(matrix[startRow][i]);
            }
            startRow++;

            // Traverse the right column
            for (int i = startRow; i < rows; i++) {
                result.push_back(matrix[i][cols - 1]);
            }
            cols--;

            // Traverse the bottom row
            if (startRow < rows) {
                for (int i = cols - 1; i >= startCol; i--) {
                    result.push_back(matrix[rows - 1][i]);
                }
                rows--;
            }

            // Traverse the left column
            if (startCol < cols) {
                for (int i = rows - 1; i >= startRow; i--) {
                    result.push_back(matrix[i][startCol]);
                }
                startCol++;
            }
        }

        return result;
    }
};
```

