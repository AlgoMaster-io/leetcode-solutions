# [Leetcode 1937: Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost/)

## Solutions
- [Solution 1: Brute Force Approach](#solution-1-brute-force-approach)
- [Solution 2: Dynamic Programming with Left and Right Traversals](#solution-2-dynamic-programming-with-left-and-right-traversals)

---

### Solution 1: Brute Force Approach

#### Intuition
The brute force solution involves iterating over each potential starting point on the first row and calculating the total score by choosing points on the subsequent rows, accounting for the cost subtraction. This approach is inefficient because it necessitates recalculating possible scores for each potential start.

#### Steps
1. Start iterating from the top-most row of the matrix.
2. For each element in the current row, calculate the score for all possible subsequent columns in the next row, subtracting the column distance cost.
3. Return the maximum score calculated.

```cpp
class Solution {
public:
    long long maxPoints(vector<vector<int>>& points) {
        int rows = points.size();
        int cols = points[0].size();
        
        // Initialize vector to store previous row scores
        vector<long long> prev(cols, 0);
        
        // Populate initial values for the dp approach
        for (int j = 0; j < cols; j++) {
            prev[j] = points[0][j];
        }
        
        // Start processing from the second row
        for (int i = 1; i < rows; i++) {
            vector<long long> current(cols, 0);
            for (int j = 0; j < cols; j++) {
                // Calculate maximum score achievable for the current element
                long long max_score = LLONG_MIN;
                for (int k = 0; k < cols; k++) {
                    max_score = max(max_score, prev[k] - abs(j - k) + points[i][j]);
                }
                current[j] = max_score;
            }
            prev = current; // Update previous row values
        }
        
        // Get the maximum score from the last row calculated
        return *max_element(prev.begin(), prev.end());
    }
};
```

**Time Complexity:** O(r * c * c), where r is the number of rows and c is the number of columns.  
**Space Complexity:** O(c), storing results for the current and previous rows.

---

### Solution 2: Dynamic Programming with Left and Right Traversals

#### Intuition
By dynamically programming the problem, we can attain an optimal solution using the concept of pre-calculating the maximum possible scores in a left-right traversal manner. Each cell in the current row will utilize pre-calculated maximum left and right scores from the previous row.

#### Steps
1. Initialize a DP vector storing the first row's values.
2. For each subsequent row, compute left and right maximum scores that include penalties for column changes. 
3. Use the left and right maximum values to compute the score for each element efficiently.
4. Return the maximum value from the last computed row.

```cpp
class Solution {
public:
    long long maxPoints(vector<vector<int>>& points) {
        int rows = points.size();
        int cols = points[0].size();
        
        vector<long long> dp(cols, 0);
        
        // Initialize DP table with the first row values
        for (int j = 0; j < cols; j++) {
            dp[j] = points[0][j];
        }
        
        // Process row by row
        for (int i = 1; i < rows; i++) {
            // Compute the left and right maximum arrays
            vector<long long> left(cols, 0);
            vector<long long> right(cols, 0);
            
            left[0] = dp[0];
            for (int j = 1; j < cols; j++) {
                left[j] = max(left[j-1] - 1, dp[j]);
            }
            
            right[cols-1] = dp[cols-1];
            for (int j = cols-2; j >= 0; j--) {
                right[j] = max(right[j+1] - 1, dp[j]);
            }
            
            // Update the DP table with the max score for the current row
            for (int j = 0; j < cols; j++) {
                dp[j] = points[i][j] + max(left[j], right[j]);
            }
        }
        
        // Return the maximum score from the last row
        return *max_element(dp.begin(), dp.end());
    }
};
```

**Time Complexity:** O(r * c), since each row computations for left and right traversals take linear time.  
**Space Complexity:** O(c), due to the use of arrays for storing previous row's values.

---

These solutions cover both the naive and the optimal approaches to solve the problem of finding the maximum number of points with cost. The efficient approach leverages dynamic programming in a strategic way to minimize computations by pre-computing useful information in left-right traversals.

