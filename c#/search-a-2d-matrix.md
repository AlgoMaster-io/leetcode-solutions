# [Leetcode 74: Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Binary Search Approach - Row-wise](#binary-search-approach---row-wise)
3. [Optimized Binary Search Approach - 1D Binary Search](#optimized-binary-search-approach---1d-binary-search)

---

### Brute Force Approach

#### Intuition:
The simplest way to solve this problem is to scan through each element of the matrix and check if it matches the target. This method uses a straightforward, row-by-row and column-by-column traversal to find the target.

#### Implementation:

```csharp
public bool SearchMatrix(int[][] matrix, int target) {
    int rows = matrix.Length;
    int cols = matrix[0].Length;

    // Traverse every element in the matrix
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            // Check if the current element is equal to the target
            if (matrix[i][j] == target) {
                return true;
            }
        }
    }

    // Return false if the target is not found
    return false;
}
```

#### Time Complexity:
- O(m * n), where m is the number of rows and n is the number of columns in the matrix.

#### Space Complexity:
- O(1), as no additional space is used.

---

### Binary Search Approach - Row-wise

#### Intuition:
Given that each row is sorted, we can utilize binary search for each row. This will reduce the search time within a row from \(O(n)\) to \(O(\log n)\).

#### Implementation:

```csharp
public bool SearchMatrix(int[][] matrix, int target) {
    int rows = matrix.Length;
    int cols = matrix[0].Length;

    for (int i = 0; i < rows; i++) {
        // Apply binary search on each row
        int start = 0;
        int end = cols - 1;

        while (start <= end) {
            int mid = start + (end - start) / 2;

            // Check if the middle element is the target
            if (matrix[i][mid] == target) {
                return true;
            }
            // If target is greater, ignore the left half
            else if (matrix[i][mid] < target) {
                start = mid + 1;
            }
            // If target is smaller, ignore the right half
            else {
                end = mid - 1;
            }
        }
    }

    return false;
}
```

#### Time Complexity:
- O(m * log n), where m is the number of rows and n is the number of columns.

#### Space Complexity:
- O(1), as we use a constant amount of extra space.

---

### Optimized Binary Search Approach - 1D Binary Search

#### Intuition:
Treat the matrix as a sorted 1D array by considering each row to be laid end-to-end. Since the matrix is sorted in total, perform a single binary search over the "virtual" 1D array which spans from 0 to \(m \times n - 1\).

#### Implementation:

```csharp
public bool SearchMatrix(int[][] matrix, int target) {
    int rows = matrix.Length;
    int cols = matrix[0].Length;

    int start = 0;
    int end = rows * cols - 1;

    // Perform binary search on the imaginary 1D array
    while (start <= end) {
        int mid = start + (end - start) / 2;
        // Calculate mid row and column index
        int midValue = matrix[mid / cols][mid % cols];

        // Check if the middle element is the target
        if (midValue == target) {
            return true;
        }
        // If target is greater, ignore the left half
        else if (midValue < target) {
            start = mid + 1;
        }
        // If target is smaller, ignore the right half
        else {
            end = mid - 1;
        }
    }

    return false;
}
```

#### Time Complexity:
- O(log(m * n)), which simplifies to O(log m + log n).

#### Space Complexity:
- O(1), as no additional space is used beyond a few integer variables.

--- 

Each approach improves upon the previous one, culminating in an efficient solution that leverages the properties of binary search to significantly reduce time complexity.

