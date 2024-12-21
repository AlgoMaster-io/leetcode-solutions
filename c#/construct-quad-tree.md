# [LeetCode Problem 427: Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Table of Contents
- [Approach 1: Recursive Solution](#approach-1)
- [Approach 2: Improved Recursive Solution with Early Stop Condition](#approach-2)

## Approach 1: Recursive Solution

### Intuition

The problem is about constructing a Quad Tree from a 2D grid. A Quad Tree is a tree data structure in which each node has exactly four children. The grid is a 2^n x 2^n matrix.

#### Basic Concepts:
1. **Leaf Node**: A node is a leaf if all values in the 2D area it represents are the same.
2. **Recursive Division**: The grid is recursively divided into four quadrants until uniform areas (leaf nodes) are obtained.

### Steps
1. **Base Case**: If the current matrix size is 1x1 or it has uniform values, create and return a leaf node.
2. **Recursive Division**: If not, recursively divide the grid into four quadrants.
3. **Merge Results**: Build an internal node with the four subdivisions as children.

### Time Complexity
The time complexity is recursive and for a grid size of \(n \times n\) (where \(n = 2^k\)), it is O(n^2).

### Space Complexity
The space complexity is O(n^2) due to the storage used by the recursion stack in the worst case.

### Code
```csharp
/*
// Definition for a QuadTree node.
public class Node {
    public bool val;
    public bool isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
    
    public Node() {}
    
    public Node(bool _val, bool _isLeaf, Node _topLeft, Node _topRight, Node _bottomLeft, Node _bottomRight) {
        val = _val;
        isLeaf = _isLeaf;
        topLeft = _topLeft;
        topRight = _topRight;
        bottomLeft = _bottomLeft;
        bottomRight = _bottomRight;
    }
}
*/

public class Solution {
    public Node Construct(int[][] grid) {
        return Construct(grid, 0, 0, grid.Length);
    }
    
    private Node Construct(int[][] grid, int row, int col, int size) {
        // Base case: if the size is 1, current grid is a leaf
        if (size == 1) {
            return new Node(grid[row][col] == 1, true, null, null, null, null);
        }
        
        // Divide the grid into four parts
        int newSize = size / 2;
        Node topLeft = Construct(grid, row, col, newSize);
        Node topRight = Construct(grid, row, col + newSize, newSize);
        Node bottomLeft = Construct(grid, row + newSize, col, newSize);
        Node bottomRight = Construct(grid, row + newSize, col + newSize, newSize);
        
        // If all four nodes are leaves and have the same value
        if (topLeft.isLeaf && topRight.isLeaf && bottomLeft.isLeaf && bottomRight.isLeaf &&
            topLeft.val == topRight.val && topRight.val == bottomLeft.val && bottomLeft.val == bottomRight.val) {
            return new Node(topLeft.val, true, null, null, null, null);
        } else {
            // Otherwise, create an internal node
            return new Node(true, false, topLeft, topRight, bottomLeft, bottomRight);
        }
    }
}
```

## Approach 2: Improved Recursive Solution with Early Stop Condition

### Intuition

This approach improves upon the basic recursive solution by adding an early stop condition. If a part of the grid contains the same value, we can immediately create a leaf node, reducing the depth of recursion.

### Steps
1. **Check Uniformity**: Before dividing the grid, check if the current region is uniform.
2. **Divide and Conquer**: If not uniform, continue as in the previous approach.
3. **Optimization**: Early stopping if a quadrant is uniform reduces unnecessary recursive calls.

### Time Complexity
The time complexity remains O(n^2), but in practice, it can be faster due to early stopping.

### Space Complexity
The space complexity is O(n^2) due to the recursion stack in the worst case.

### Code
```csharp
public class Solution {
    public Node Construct(int[][] grid) {
        return Construct(grid, 0, 0, grid.Length);
    }
    
    private Node Construct(int[][] grid, int row, int col, int size) {
        // Check if the current region is uniform
        if (IsUniform(grid, row, col, size)) {
            return new Node(grid[row][col] == 1, true, null, null, null, null);
        }
        
        // If not uniform, divide the grid into four parts
        int newSize = size / 2;
        Node topLeft = Construct(grid, row, col, newSize);
        Node topRight = Construct(grid, row, col + newSize, newSize);
        Node bottomLeft = Construct(grid, row + newSize, col, newSize);
        Node bottomRight = Construct(grid, row + newSize, col + newSize, newSize);
        
        return new Node(true, false, topLeft, topRight, bottomLeft, bottomRight);
    }

    private bool IsUniform(int[][] grid, int row, int col, int size) {
        int val = grid[row][col];
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) {
                if (grid[row + i][col + j] != val) return false;
            }
        }
        return true;
    }
}
```

This approach is generally faster in scenarios where large portions of the grid are uniform, as it reduces unnecessary recursive processing.

