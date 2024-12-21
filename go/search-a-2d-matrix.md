# [Leetcode 74: Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

## Approaches:
- [Approach 1: Linear Search](#approach-1-linear-search)
- [Approach 2: Binary Search on Every Row](#approach-2-binary-search-on-every-row)
- [Approach 3: Treat as a 1D Sorted Array](#approach-3-treat-as-a-1d-sorted-array)

### Approach 1: Linear Search

**Intuition:**

The simplest approach is to iterate over each element in the 2D matrix and check if it matches the target. This solution is straightforward but inefficient.

**Steps:**

1. Traverse through each row.
2. For each row, traverse through each column and check if the element is equal to the target.

```go
func searchMatrix(matrix [][]int, target int) bool {
    // Traverse each row of the matrix
    for _, row := range matrix {
        // Traverse each element in the row
        for _, element := range row {
            // Check if the current element is equal to the target
            if element == target {
                return true
            }
        }
    }
    // Target not found in the matrix
    return false
}
```

**Time Complexity:** O(m * n) where m = number of rows, n = number of columns.

**Space Complexity:** O(1), no extra space used.

---

### Approach 2: Binary Search on Every Row

**Intuition:**

Since each row is sorted in ascending order, we can apply binary search on each row. This would reduce the time complexity significantly compared to a linear search.

**Steps:**

1. Traverse each row of the matrix.
2. Perform a binary search on the current row to find if the target is present.

```go
func searchMatrix(matrix [][]int, target int) bool {
    for _, row := range matrix {
        // Perform binary search within the row
        left, right := 0, len(row)-1
        for left <= right {
            mid := left + (right-left)/2
            if row[mid] == target {
                return true
            } else if row[mid] < target {
                left = mid + 1
            } else {
                right = mid - 1
            }
        }
    }
    return false
}
```

**Time Complexity:** O(m * log n) where m = number of rows, n = number of columns.

**Space Complexity:** O(1), no extra space used.

---

### Approach 3: Treat as a 1D Sorted Array

**Intuition:**

Given that the whole matrix can be thought of as being fully sorted from the top-left element to the bottom-right element (since each row is sorted and the first element of each row is greater than the last element of the previous row), we can treat this problem as a binary search over a 1D array.

**Steps:**

1. Calculate the number of rows `m` and number of columns `n`.
2. Set two pointers `left` and `right` at the start and end of the imaginary 1D array (0 and m*n - 1).
3. Use binary search to navigate through the imaginary array:
   - Calculate the middle element index in terms of the 1D array and convert it to the 2D matrix indices.
   - If the middle element matches the target, return true.
   - If the middle element is less than the target, move the left pointer.
   - If it is greater, move the right pointer.

```go
func searchMatrix(matrix [][]int, target int) bool {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return false
    }
    rows, cols := len(matrix), len(matrix[0])
    left, right := 0, rows*cols-1
    
    for left <= right {
        mid := left + (right-left)/2
        midValue := matrix[mid/cols][mid%cols]
        
        if midValue == target {
            return true
        } else if midValue < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return false
}
```

**Time Complexity:** O(log(m * n)) where m = number of rows, n = number of columns.

**Space Complexity:** O(1), no extra space used.

This approach is optimal as it effectively treats the problem as a single binary search over a sorted array.

