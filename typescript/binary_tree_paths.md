# 257. [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

## Approach 1: Recursive Depth-First Search (DFS)

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

function binaryTreePaths(root: TreeNode | null): string[] {
    const result: string[] = [];
    if (root) {
        dfs(root, "", result);
    }
    return result;
}

function dfs(node: TreeNode, path: string, result: string[]): void {
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
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(n), for the stack and path storage

function binaryTreePaths(root: TreeNode | null): string[] {
    const result: string[] = [];
    if (root === null) {
        return result; // Return empty result if the tree is empty
    }

    const stack: TreeNode[] = [];
    const paths: string[] = [];

    stack.push(root);
    paths.push(root.val.toString());

    while (stack.length > 0) {
        const currentNode = stack.pop()!;
        const currentPath = paths.pop()!;

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
typescript
```typescript
// Time Complexity: O(n), where n is the number of nodes in the tree
// Space Complexity: O(h), where h is the height of the tree for recursion stack

function binaryTreePaths(root: TreeNode | null): string[] {
    const result: string[] = [];
    if (root === null) {
        return result; // Return empty result if the tree is empty
    }
    backtrack(root, "", result);
    return result;
}

function backtrack(node: TreeNode, path: string, result: string[]): void {
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
    path = path.slice(0, len);
}
```

