# [Leetcode 74: Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

## Approaches
- [Approach 1: Flatten the Matrix and Use Binary Search](#approach-1-flatten-the-matrix-and-use-binary-search)
- [Approach 2: Binary Search without Flattening](#approach-2-binary-search-without-flattening)

## Approach 1: Flatten the Matrix and Use Binary Search

### Intuition
The matrix is effectively a sorted list of integers if you view the entire matrix consecutively. By using binary search, we can flatten the matrix (treat it as a 1D sorted array) and find the target value efficiently.

### Implementation
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix[0].empty()) return false;
        
        int rows = matrix.size();
        int cols = matrix[0].size();
        
        int left = 0, right = rows * cols - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            // Convert mid index into matrix's row and column
            int mid_value = matrix[mid / cols][mid % cols];
            
            if (mid_value == target) {
                return true;
            } else if (mid_value < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return false;
    }
};
```

### Complexity Analysis
- **Time Complexity:** \(O(\log(m \times n))\) where \(m\) is the number of rows and \(n\) is the number of columns. Binary search on a 1D array.
- **Space Complexity:** \(O(1)\). Constant space as there's no additional data structure used apart from variables.

## Approach 2: Binary Search without Flattening

### Intuition
Instead of performing binary search on a flattened view, we can simulate the 2D matrix using 1D indices directly. This allows us to perform binary search effectively in-place without flattening.

### Implementation
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.empty() || matrix[0].empty()) return false;
        
        int rows = matrix.size();
        int cols = matrix[0].size();
        
        int left = 0, right = rows * cols - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int mid_value = matrix[mid / cols][mid % cols];
            
            if (mid_value == target) {
                return true;
            } else if (mid_value < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return false;
    }
};
```

### Complexity Analysis
- **Time Complexity:** \(O(\log(m \times n))\). Binary search on a conceptual straight line of the matrix.
- **Space Complexity:** \(O(1)\). Again, only a few integer variables are utilized.

Both approaches essentially utilize the power of binary search by treating the matrix as a sorted list of integers, with the only difference being the conceptual visualization of how the search space is represented and visualized.

