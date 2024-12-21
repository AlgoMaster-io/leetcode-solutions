# [Leetcode 54: Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

## Approaches:
1. [Basic Simulation Approach](#basic-simulation-approach)
2. [Optimized Direction Change Approach](#optimized-direction-change-approach)

---

### Basic Simulation Approach

**Intuition:**

The problem is about collecting numbers in a matrix in a defined "spiral order". We can simulate this process by keeping track of boundaries that govern the visible "layer" of the matrix being iterated upon. Initially, these boundaries correspond to the full extent of the matrix, and they contract as we pick up elements in spiral fashion.

We maintain four boundaries:
- Top boundary: starts at the first row and moves downward.
- Bottom boundary: starts at the last row and moves upward.
- Left boundary: starts at the first column and moves rightward.
- Right boundary: starts at the last column and moves leftward.

By traversing these boundaries in the correct sequence, we can extract the elements of the matrix in spiral order.

**Steps:**

1. Traverse the top row from left boundary to right boundary, then move the top boundary downward.
2. Traverse the right column from top boundary to bottom boundary, then move the right boundary leftward.
3. Traverse the bottom row from right boundary to left boundary, then move the bottom boundary upward.
4. Traverse the left column from bottom boundary to top boundary, then move the left boundary rightward.
5. Repeat until all boundaries meet or cross each other.

**Code:**

```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> result = new ArrayList<>();
    if (matrix == null || matrix.length == 0) {
        return result;
    }
    
    int top = 0;
    int bottom = matrix.length - 1;
    int left = 0;
    int right = matrix[0].length - 1;
    
    while (top <= bottom && left <= right) {
        // Traverse from left to right on the current top row
        for (int i = left; i <= right; i++) {
            result.add(matrix[top][i]);
        }
        top++;  // Shift the top boundary down
        
        // Traverse from top to bottom on the current right column
        for (int i = top; i <= bottom; i++) {
            result.add(matrix[i][right]);
        }
        right--;  // Shift the right boundary left
        
        // Traverse from right to left on the current bottom row if top <= bottom
        if (top <= bottom) {
            for (int i = right; i >= left; i--) {
                result.add(matrix[bottom][i]);
            }
            bottom--;  // Shift the bottom boundary up
        }
        
        // Traverse from bottom to top on the current left column if left <= right
        if (left <= right) {
            for (int i = bottom; i >= top; i--) {
                result.add(matrix[i][left]);
            }
            left++;  // Shift the left boundary right
        }
    }
    
    return result;
}
```

**Complexity Analysis:**

- **Time Complexity:** O(m * n), where m is the number of rows and n is the number of columns. Every element in the matrix is visited exactly once.
- **Space Complexity:** O(1), excluding the space used by the output list.

---

### Optimized Direction Change Approach

**Intuition:**

A more structured version of the basic approach relies on cycling through four directions: right, down, left, and up. We can simplify by using a direction index to manage changes in direction and move uniformly until a boundary condition stops us.

**Steps:**

1. Define coordinates changes for directions: `(0,1)` for right, `(1,0)` for down, `(0,-1)` for left, `(-1,0)` for up.
2. Use an index to rotate through these directions as necessary.
3. For each movement action, validate if the destination is within unvisited cells.
4. Adjust direction and mark cell as visited when hitting a boundary.

**Code:**

```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> result = new ArrayList<>();
    if (matrix == null || matrix.length == 0) {
        return result;
    }
    
    int m = matrix.length;
    int n = matrix[0].length;
    boolean[][] visited = new boolean[m][n];
    int[] dr = {0, 1, 0, -1}; // direction control array for row
    int[] dc = {1, 0, -1, 0}; // direction control array for column
    int r = 0, c = 0, di = 0; // start from top-left corner

    for (int i = 0; i < m * n; i++) {
        result.add(matrix[r][c]);
        visited[r][c] = true;
        int newR = r + dr[di];
        int newC = c + dc[di];
        
        // If the new coordinates are out of bounds or already visited, switch direction
        if (newR >= 0 && newR < m && newC >= 0 && newC < n && !visited[newR][newC]) {
            r = newR; // move to the next cell in current direction
            c = newC;
        } else {
            di = (di + 1) % 4; // change the direction
            r += dr[di];
            c += dc[di];
        }
    }
    
    return result;
}
```

**Complexity Analysis:**

- **Time Complexity:** O(m * n), since each cell of the matrix is visited exactly once.
- **Space Complexity:** O(m * n), due to the visited matrix tracking visited cells.

