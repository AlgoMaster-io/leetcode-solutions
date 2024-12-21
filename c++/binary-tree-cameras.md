# [Leetcode 968: Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/)

## Approaches:
1. [Greedy DFS approach](#greedy-dfs-approach)
2. [Greedy via State Using Post-Order Traversal](#greedy-via-state-using-post-order-traversal)

---

### Greedy DFS approach

#### Intuition:
To solve the problem of placing the minimum number of cameras in a binary tree such that each node is monitored, a logical division into states is helpful. Each node can be treated as a part of different states - each state being either covered by a camera, needing a camera, or even being a place where a camera is placed. The idea here is to perform a postorder DFS traversal so that we decide the placement of a camera considering the subtrees first.

1. A node will require a camera if either of its children is not covered.
2. A node is covered if either a camera is placed at it, or any of its children have a camera.
3. A node can be a place for a camera if any of its children require a camera.

#### Implementation:
```cpp
class Solution {
public:
    int minCameraCover(TreeNode* root) {
        // If after the traversal root is in a state of needing a camera, increment the count.
        return (dfs(root) == 0 ? 1 : 0) + cameras;
    }
    
private:
    int cameras = 0;
    
    // This will return the state of the node:
    // 0 -> Needs a camera
    // 1 -> Covered
    // 2 -> Has a camera
    int dfs(TreeNode* node) {
        if (!node) return 1; // Null nodes are covered, returning state 1.

        int left = dfs(node->left);   // State of left child
        int right = dfs(node->right); // State of right child
        
        // If any child needs a camera, place a camera here and return the camera state.
        if (left == 0 || right == 0) {
            cameras++;
            return 2;
        }
        
        // If any child has a camera, this node is covered.
        if (left == 2 || right == 2) return 1;
        
        // Otherwise, this node needs a camera.
        return 0;
    }
};
```

#### Complexity Analysis:
- **Time Complexity**: O(N), where N is the number of nodes in the tree since we are visiting each node exactly once in the DFS traversal.
- **Space Complexity**: O(H), where H is the height of the tree, due to the recursion stack.

---

### Greedy via State Using Post-Order Traversal

#### Intuition:
This approach builds upon the same concept as the previous approach, with enhanced readability through distinct state explanations. The states guide the placement decision, ensuring each node is adequately monitored.

#### Implementation:
The complexity and results would be identical to the first approach. Generally, such implementations will prompt adaptations focused on stylistic or syntactical preferences over core logical shifts.

The implemented method might merge subtly differ in how it handles node states or function return values, but fundamentally they solve the problem relying upon the same principles of greedy postorder traversal ensuring efficient camera distribution.

### Conclusion

Efficient placement of cameras in a binary tree primarily relies on minimizing placements while ensuring coverage. The Greedy DFS approach effectively provides the means to achieve the optimal count through strategic decision-making across node states. This tactic is not only intuitive due to its logical mapping but also efficient in computational resource utilization.

