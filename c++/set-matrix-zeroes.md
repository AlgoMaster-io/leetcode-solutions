# [Leetcode 73: Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

## Table of Contents

1. [Approach 1: Additional Space Approach](#approach-1-additional-space-approach)
2. [Approach 2: Optimized In-place with Two Flags](#approach-2-optimized-in-place-with-two-flags)
3. [Approach 3: Most Optimal In-place Solution](#approach-3-most-optimal-in-place-solution)

---

## Approach 1: Additional Space Approach

### Intuition
The most straightforward approach involves using additional space. We create additional arrays to store information about which rows and columns should be zeroed. This approach may not be the most space-efficient, but it helps with understanding the problem and is an easy-to-implement solution.

### Steps
1. Create two boolean arrays, `row` and `col`, to track which rows and columns contain zeros.
2. Iterate through the matrix, marking the corresponding indices in the `row` and `col` arrays if a zero is found in the matrix.
3. Iterate again: for each entry in the matrix, set it to zero if its row or column was marked in the previous iteration.

### Code
```cpp
#include <vector>
using namespace std;

void setZeroes(vector<vector<int>>& matrix) {
    int m = matrix.size(), n = matrix[0].size();
    vector<bool> row(m, false), col(n, false);
    
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            // If a zero is found, mark the row and column
            if (matrix[i][j] == 0) {
                row[i] = true;
                col[j] = true;
            }
        }
    }
    
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            // If the row or column is marked, set the element to zero
            if (row[i] || col[j]) {
                matrix[i][j] = 0;
            }
        }
    }
}
```

### Complexity
- **Time Complexity**: O(m * n), where m is the number of rows and n is the number of columns.
- **Space Complexity**: O(m + n) due to the additional row and col arrays.

---

## Approach 2: Optimized In-place with Two Flags

### Intuition
To reduce space usage, we can use two flags to determine if the first row and the first column should be zeroed. The rest of the matrix can be used to store state information.

### Steps
1. Use two boolean variables, `rowZero` and `colZero`, to determine if the first row or column should be zeroed.
2. Traverse the matrix, using the top row and left column to mark zero states for other rows and columns.
3. Finally, iterate over the matrix again to update the entries according to these markings.

### Code
```cpp
#include <vector>
using namespace std;

void setZeroes(vector<vector<int>>& matrix) {
    int m = matrix.size(), n = matrix[0].size();
    bool rowZero = false, colZero = false;

    // Check if first row or column has zero
    for (int i = 0; i < m; ++i) if (matrix[i][0] == 0) colZero = true;
    for (int j = 0; j < n; ++j) if (matrix[0][j] == 0) rowZero = true;

    // Use the first row and column to mark zero state
    for (int i = 1; i < m; ++i) {
        for (int j = 1; j < n; ++j) {
            if (matrix[i][j] == 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }

    // Update the matrix using the marks from the first row and column
    for (int i = 1; i < m; ++i) {
        for (int j = 1; j < n; ++j) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                matrix[i][j] = 0;
            }
        }
    }

    // Zero first row/column if necessary
    if (colZero) for (int i = 0; i < m; ++i) matrix[i][0] = 0;
    if (rowZero) for (int j = 0; j < n; ++j) matrix[0][j] = 0;
}
```

### Complexity
- **Time Complexity**: O(m * n).
- **Space Complexity**: O(1), additional space is constant.

---

## Approach 3: Most Optimal In-place Solution

### Intuition
Building on the previous approach, we optimize further by reducing unnecessary iterations, leveraging the matrix's structure itself to store the required state.

### Steps
1. First, determine zero states of the first row and column in a single pass.
2. Use these rows and columns to manage state information for other portions of the matrix as before.
3. Update the matrix according to these markings simultaneously, thus doing fewer passes and thus potentially faster execution.

### Code
```cpp
#include <vector>
using namespace std;

void setZeroes(vector<vector<int>>& matrix) {
    int m = matrix.size(), n = matrix[0].size();
    bool firstRowZero = false;

    for (int j = 0; j < n; ++j) {
        if (matrix[0][j] == 0) {
            firstRowZero = true;
            break;
        }
    }

    for (int i = 1; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (matrix[i][j] == 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }

    for (int i = 1; i < m; ++i) {
        for (int j = 1; j < n; ++j) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                matrix[i][j] = 0;
            }
        }
    }

    if (matrix[0][0] == 0) { 
        for (int i = 0; i < m; ++i) {
            matrix[i][0] = 0;
        }
    }

    if (firstRowZero) {
        for (int j = 0; j < n; ++j) {
            matrix[0][j] = 0;
        }
    }
}
```

### Complexity
- **Time Complexity**: O(m * n).
- **Space Complexity**: O(1), uses no additional space besides auxiliary variables.

