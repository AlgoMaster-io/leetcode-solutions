[Leetcode 1277: Count Square Submatrices with All Ones](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)

### Approaches
- [Brute Force Approach](#brute-force-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)

## Brute Force Approach

### Intuition:
The brute force approach involves iterating over every element in the matrix and checking if a square of a particular size can be formed with the top-left corner at the current element. If a square can be formed, increment the count of squares. This involves checking the boundaries of each square size.

### Detailed Comments:
- For each cell in the matrix, consider it as the top-left corner of a potential square.
- Try to build squares of increasing sizes originating from this corner.
- Use nested loops to check boundaries because a square of size `k` means all cells from `(i, j)` to `(i+k-1, j+k-1)` should be 1.
- Increment the count each time a valid square is found.

### Code:
```cpp
#include <vector>
#include <algorithm>
using namespace std;

int countSquares(vector<vector<int>>& matrix) {
    int rows = matrix.size(), cols = matrix[0].size(), count = 0;
    
    // Iterate over each cell in the matrix.
    for(int i = 0; i < rows; i++) {
        for(int j = 0; j < cols; j++) {
            // Check for squares starting at (i, j).
            if(matrix[i][j] == 1) {
                // Start with a single 1x1 square.
                count++;
                // Try to expand the square size.
                int maxSize = min(rows - i, cols - j);
                for(int size = 1; size < maxSize; size++) {
                    bool isSquare = true;
                    for(int k = 0; k <= size; k++) {
                        if(matrix[i + size][j + k] == 0 || matrix[i + k][j + size] == 0) {
                            isSquare = false;
                            break;
                        }
                    }
                    if(isSquare) count++;
                    else break;
                }
            }
        }
    }
    
    return count;
}
```

### Time Complexity:
- The time complexity is `O(n * m * min(n, m))` because for every cell `(n * m)`, in the worst case, we check up to `min(n, m)` additional cells.

### Space Complexity:
- The space complexity is `O(1)` as we're not using any extra data structures proportional to input size.

## Dynamic Programming Approach

### Intuition:
Using dynamic programming, the number of squares with the bottom-right corner at a particular cell can be determined by the minimum of top, left, and top-left values if the current cell is 1. This allows counting squares efficiently because each DP cell value will carry the number of possible maximal squares with that corner.

### Detailed Comments:
- Create a DP table `dp` of the same size as `matrix`.
- If `matrix[i][j]` is 1, then `dp[i][j]` will be the minimal extension of a square from its adjacent top, left, and top-left DP cells.
- Sum up all DP values to get the total number of squares.

### Code:
```cpp
#include <vector>
#include <algorithm>
using namespace std;

int countSquares(vector<vector<int>>& matrix) {
    int rows = matrix.size(), cols = matrix[0].size(), count = 0;
    
    // Initialize the DP array with the same size as matrix
    vector<vector<int>> dp(rows, vector<int>(cols, 0));
    
    for(int i = 0; i < rows; i++) {
        for(int j = 0; j < cols; j++) {
            // If it's the first row or column, DP value is same as matrix cell
            if(i == 0 || j == 0) {
                dp[i][j] = matrix[i][j];
            }
            // Else calculate the dp value
            else if(matrix[i][j] == 1) {
                dp[i][j] = 1 + min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]});
            }
            // Increment count by the value in dp
            count += dp[i][j];
        }
    }
    
    return count;
}
```

### Time Complexity:
- The time complexity is `O(n * m)` since we only make a single pass through the matrix.

### Space Complexity:
- The space complexity is `O(n * m)` due to the DP table, which is the same size as the input matrix. 

By employing dynamic programming, we have crafted an efficient solution that allows us to avoid redundant computations and manage problem constraints effectively.


