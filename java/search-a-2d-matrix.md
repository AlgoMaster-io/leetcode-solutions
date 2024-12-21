# [Leetcode 74: Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

## Approaches
- [Approach 1: Brute Force Linear Search](#approach-1-brute-force-linear-search)
- [Approach 2: Binary Search on 2D Matrix](#approach-2-binary-search-on-2d-matrix)
- [Approach 3: Treat 2D Matrix as 1D Array](#approach-3-treat-2d-matrix-as-1d-array)

---

## Approach 1: Brute Force Linear Search

### Intuition
The simplest way to search for a target in a 2D matrix is to iterate through each element until we find the target or exhaust all possibilities. This approach mimics a direct linear search, which is straightforward but inefficient for large matrices.

### Solution
```java
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        // Iterate over each row
        for (int i = 0; i < matrix.length; i++) {
            // Iterate over each column in the current row
            for (int j = 0; j < matrix[0].length; j++) {
                // Check if the current element matches the target
                if (matrix[i][j] == target) {
                    return true; // Target found
                }
            }
        }
        return false; // Target not found in the matrix
    }
}
```

### Time Complexity
- O(m * n), where m is the number of rows and n is the number of columns in the matrix.

### Space Complexity
- O(1), as we are not using any extra space besides a few variables.

---

## Approach 2: Binary Search on 2D Matrix

### Intuition
Given that the rows of the matrix are sorted in non-decreasing order and each first integer of a row is greater than the last integer of the previous row, each row can independently be subjected to a binary search to determine if the target exists therein. This approach capitalizes on this sorted property, reducing the search space more efficiently than a linear scan.

### Solution
```java
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        // Base case
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return false;
        
        int m = matrix.length;
        int n = matrix[0].length;
        
        // Binary search over rows
        for (int i = 0; i < m; i++) {
            // Check if target could potentially be in the current row
            if (target >= matrix[i][0] && target <= matrix[i][n - 1]) {
                int left = 0;
                int right = n - 1;
                
                // Perform binary search in the current row
                while (left <= right) {
                    int mid = left + (right - left) / 2;
                    if (matrix[i][mid] == target) {
                        return true;
                    } else if (matrix[i][mid] < target) {
                        left = mid + 1;
                    } else {
                        right = mid - 1;
                    }
                }
            }
        }
        return false; // Target not found
    }
}
```

### Time Complexity
- O(m * log(n)), since we do a binary search (logarithmic time complexity) across each of the m rows.

### Space Complexity
- O(1), as no additional space is used besides vars.

---

## Approach 3: Treat 2D Matrix as 1D Array

### Intuition
By treating the 2D matrix as a sorted 1D array, we can perform a single binary search to find the target. The index conversion between the 1D and 2D matrix can be done using simple row and column calculations. This method fully utilizes the sorted property of the entire matrix, leading to an extremely efficient solution.

### Solution
```java
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return false;
        
        int m = matrix.length;
        int n = matrix[0].length;
        int left = 0;
        int right = m * n - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            // Calculate row and column from the mid index
            int midElement = matrix[mid / n][mid % n];
            
            if (midElement == target) {
                return true; // Target found
            } else if (midElement < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return false; // Target not found
    }
}
```

### Time Complexity
- O(log(m * n)), which simplifies to O(log(mn)) due to the single binary search on the entire matrix.

### Space Complexity
- O(1), as the solution uses only a fixed amount of extra space.

