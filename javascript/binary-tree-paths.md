# [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approaches

1. [Recursive Depth-First Search (DFS)](#recursive-depth-first-search-dfs)
2. [Iterative Depth-First Search (DFS) using Stack](#iterative-depth-first-search-dfs-using-stack)

---

## Recursive Depth-First Search (DFS)

The recursive approach utilizes depth-first search to traverse the binary tree, building paths as it explores each node. 

### Intuition

- The primary intuition is to traverse the tree recursively in a depth-first manner. 
- Starting from the root, at each node, append the node's value to the current path. 
- If the node is a leaf, finalize the current path and add it to the results. 
- If the node is not a leaf, continue to both child nodes recursively. 

### Implementation

```javascript
var binaryTreePaths = function(root) {
    // Helper function for recursive DFS
    const dfs = (node, path, result) => {
        if (!node) return; // Base case: If node is null, return
        
        path += node.val; // Append current node's value to the path

        // Check if the current node is a leaf node
        if (!node.left && !node.right) {
            result.push(path); // Add the finalized path to result
            return;
        }
        
        // If not a leaf node, append '->' and recurse on children
        if (node.left) dfs(node.left, path + '->', result);
        if (node.right) dfs(node.right, path + '->', result);
    };

    let result = [];
    dfs(root, '', result);
    return result;
};
```

### Time and Space Complexity

- **Time Complexity:** O(N), where N is the number of nodes in the tree. Each node is visited once.
- **Space Complexity:** O(H), where H is the height of the tree. This space is used by the recursion stack.

---

## Iterative Depth-First Search (DFS) using Stack

Another approach is using an explicit stack to simulate the depth-first search, which might be better suited to avoid recursion stack limit issues for very deep trees.

### Intuition

- Use a stack to simulate the recursion stack.
- Each stack element stores a node along with the path so far. 
- Process each node: if it’s a leaf, add the path to the result. 
- If not, add its children to the stack with updated paths. 

### Implementation

```javascript
var binaryTreePaths = function(root) {
    if (!root) return []; // Edge case: If root is null, return an empty list
    
    let result = [];
    // Stack will store tuples of (current node, current path)
    let stack = [[root, `${root.val}`]];

    while (stack.length > 0) {
        let [node, path] = stack.pop(); // Pop the top item (DFS)

        // If a leaf node is reached, add the path to the result
        if (!node.left && !node.right) {
            result.push(path);
        }
    
        // If there are children, add them to stack with updated paths
        if (node.right) stack.push([node.right, `${path}->${node.right.val}`]);
        if (node.left) stack.push([node.left, `${path}->${node.left.val}`]);
    }

    return result;
};
```

### Time and Space Complexity

- **Time Complexity:** O(N), where N is the number of nodes in the tree. Each node is visited once.
- **Space Complexity:** O(N), considering the storage in the stack in the worst case when the tree is skewed.

This approach is robust against very deep trees and makes handling large input sizes more feasible without risking a stack overflow due to recursive depth. It also gives an iterative approach to managing tree traversal systematically.

