# [427. Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)

## Approach 1: Recursive Division of Grid

### Solution

```cpp
// Time Complexity: O(n^2 * log n), where n is the side length of the grid
// Space Complexity: O(log n) (due to recursion stack)
class Node {
public:
    bool val;
    bool isLeaf;
    Node* topLeft;
    Node* topRight;
    Node* bottomLeft;
    Node* bottomRight;

    Node() {}

    Node(bool _val, bool _isLeaf) : val(_val), isLeaf(_isLeaf), topLeft(nullptr), topRight(nullptr), bottomLeft(nullptr), bottomRight(nullptr) {}

    Node(bool _val, bool _isLeaf, Node* _topLeft, Node* _topRight, Node* _bottomLeft, Node* _bottomRight) 
    : val(_val), isLeaf(_isLeaf), topLeft(_topLeft), topRight(_topRight), bottomLeft(_bottomLeft), bottomRight(_bottomRight) {}
};

class Solution {
public:
    Node* construct(vector<vector<int>>& grid) {
        return buildTree(grid, 0, 0, grid.size());
    }

private:
    Node* buildTree(vector<vector<int>>& grid, int x, int y, int size) {
        if (size == 1) {
            // Base case: single cell
            return new Node(grid[x][y] == 1, true, nullptr, nullptr, nullptr, nullptr);
        }

        int halfSize = size / 2;

        // Recursively divide the grid into four quadrants
        Node* topLeft = buildTree(grid, x, y, halfSize);
        Node* topRight = buildTree(grid, x, y + halfSize, halfSize);
        Node* bottomLeft = buildTree(grid, x + halfSize, y, halfSize);
        Node* bottomRight = buildTree(grid, x + halfSize, y + halfSize, halfSize);

        // Check if all four quadrants are leaves with the same value
        if (topLeft->isLeaf && topRight->isLeaf && bottomLeft->isLeaf && bottomRight->isLeaf
            && topLeft->val == topRight->val && topRight->val == bottomLeft->val
            && bottomLeft->val == bottomRight->val) {
            return new Node(topLeft->val, true, nullptr, nullptr, nullptr, nullptr); // Merge into one leaf
        }

        // Otherwise, create an internal node
        return new Node(true, false, topLeft, topRight, bottomLeft, bottomRight);
    }
};
```

