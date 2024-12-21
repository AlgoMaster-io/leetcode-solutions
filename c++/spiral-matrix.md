# [Leetcode 54: Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Solutions
- [Approach 1: Layer-by-Layer Traversal](#approach-1-layer-by-layer-traversal)

### Approach 1: Layer-by-Layer Traversal

**Intuition**:  
The idea is to traverse the matrix in a spiral order by rotating the direction of traversal whenever we reach the boundaries. We can achieve this by maintaining a boundary for each direction: top, bottom, left, right. We will continuously add elements to our result by traversing these boundaries until they converge.

**Algorithm**:  
1. Begin with the outermost layer of the matrix.
2. Traverse from left to right along the top boundary and move it one step closer to the bottom.
3. Traverse from top to bottom along the right boundary and move it one step closer to the left.
4. Check if the current layer is completely traversed, given the possibility that the layer could shrink to either one row or one column.
5. If there are still remaining elements, traverse from right to left along the bottom boundary and move it one step closer to the top.
6. Traverse from bottom to top along the left boundary and move it one step closer to the right.
7. Repeat steps 2 to 6 while adjusting the boundaries until all elements are traversed.

**Code**:
```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;
        if(matrix.empty()) return result;
        
        int top = 0; // Initialize the top boundary
        int bottom = matrix.size() - 1; // Initialize the bottom boundary
        int left = 0; // Initialize the left boundary
        int right = matrix[0].size() - 1; // Initialize the right boundary
        
        while(top <= bottom && left <= right) {
            // Traverse from left to right along the current top boundary
            for(int i = left; i <= right; ++i) {
                result.push_back(matrix[top][i]);
            }
            ++top; // Move the top boundary downwards
            
            // Traverse from top to bottom along the current right boundary
            for(int i = top; i <= bottom; ++i) {
                result.push_back(matrix[i][right]);
            }
            --right; // Move the right boundary leftwards
            
            // Check if the current boundaries have shrunk into lines
            if(top <= bottom) {
                // Traverse from right to left along the current bottom boundary
                for(int i = right; i >= left; --i) {
                    result.push_back(matrix[bottom][i]);
                }
                --bottom; // Move the bottom boundary upwards
            }
             
            if(left <= right) {
                // Traverse from bottom to top along the current left boundary
                for(int i = bottom; i >= top; --i) {
                    result.push_back(matrix[i][left]);
                }
                ++left; // Move the left boundary rightwards
            }
        }
        
        return result;
    }
};
```

**Complexity Analysis**:
- **Time Complexity**: O(m * n), where `m` is the number of rows and `n` is the number of columns. We visit each element of the matrix exactly once.
- **Space Complexity**: O(1) additional space. The output itself does not count towards the space complexity.

