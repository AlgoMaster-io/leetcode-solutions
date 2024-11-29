# 257. [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approach 1: Recursive Depth-First Search (DFS)

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

function binaryTreePaths(root) {
    const result = [];

    if (root !== null) {
        dfs(root, "", result);
    }

    return result;
}

function dfs(node, path, result) {
    // Append the current node's value to the path
    path += node.val;

    // If the current node is a leaf, add the path to the result
    if (node.left === null && node.right === null) {
        result.push(path);
        return;
    }

    // If not a leaf, continue the path with "->" and recurse
    if (node.left !== null) {
        dfs(node.left, path + "->", result);
    }
    if (node.right !== null) {
        dfs(node.right, path + "->", result);
    }
}
```

## Approach 2: Iterative Depth-First Search (Using Stack)

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n), for the stack and path storage

function binaryTreePaths(root) {
    const result = [];

    if (root === null) {
        return result; // Return empty result if the tree is empty
    }

    const stack = [];
    const paths = [];
    stack.push(root);
    paths.push(root.val.toString());

    while (stack.length > 0) {
        const currentNode = stack.pop();
        const currentPath = paths.pop();

        // If the current node is a leaf, add the path to the result
        if (currentNode.left === null && currentNode.right === null) {
            result.push(currentPath);
        }

        // If not a leaf, push children to the stack with updated paths
        if (currentNode.right !== null) {
            stack.push(currentNode.right);
            paths.push(currentPath + "->" + currentNode.right.val);
        }
        if (currentNode.left !== null) {
            stack.push(currentNode.left);
            paths.push(currentPath + "->" + currentNode.left.val);
        }
    }

    return result;
}
```

## Approach 3: Backtracking

### Solution
```javascript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

function binaryTreePaths(root) {
    const result = [];

    if (root === null) {
        return result; // Return empty result if the tree is empty
    }

    backtrack(root, "", result);
    return result;
}

function backtrack(node, path, result) {
    const len = path.length;
    path += node.val;

    // If the current node is a leaf, add the path to the result
    if (node.left === null && node.right === null) {
        result.push(path);
    } else {
        // If not a leaf, continue the path
        path += "->";
        if (node.left !== null) {
            backtrack(node.left, path, result);
        }
        if (node.right !== null) {
            backtrack(node.right, path, result);
        }
    }

    // Backtrack to remove the current node's value and "->"
    path = path.substring(0, len);
}
```

