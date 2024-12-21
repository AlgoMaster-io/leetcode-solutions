[Leetcode Problem 257: Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)

### Solutions
1. [Depth First Search (DFS) with Recursion](#depth-first-search-dfs-with-recursion)
2. [Depth First Search (DFS) Iterative using Stack](#depth-first-search-dfs-iterative-using-stack)

---

#### Depth First Search (DFS) with Recursion

**Intuition:**

The problem requires us to find all paths from the root to the leaves in a binary tree. A recursive DFS approach is well-suited for this because it naturally explores all paths from the root to the leaves. We recursively explore all nodes, keeping track of the current path. Once we reach a leaf node, we add the path to our list of results.

```typescript
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
    const paths: string[] = [];
    
    // Helper function for recursive DFS
    const dfs = (node: TreeNode | null, path: string) => {
        if (node !== null) {
            path += node.val.toString();
            
            // If a leaf node is reached, add the path to results
            if (!node.left && !node.right) {
                paths.push(path);
            } else {
                path += '->'; // Add separator for path
                // Recursively explore left and right subtrees
                dfs(node.left, path);
                dfs(node.right, path);
            }
        }
    }
    
    // Start DFS from root
    dfs(root, '');
    return paths;
}
```

**Time Complexity:** \(O(N)\), where \(N\) is the number of nodes, as each node is visited once.

**Space Complexity:** \(O(H)\), where \(H\) is the height of the tree for the recursion stack. In the worst case, the space complexity can be \(O(N)\) if the tree is skewed.

---

#### Depth First Search (DFS) Iterative using Stack

**Intuition:**

An iterative approach can replace the recursive method by using an explicit stack to simulate the call stack. We push pairs of the current node and the path string onto the stack. If a node is a leaf, we add the path to our result. Otherwise, we continue pushing its children along with the updated path.

```typescript
function binaryTreePathsIterative(root: TreeNode | null): string[] {
    if (!root) return [];

    const paths: string[] = [];
    const stack: [TreeNode, string][] = [[root, ""]];

    while (stack.length > 0) {
        const [node, path] = stack.pop()!;
        
        const newPath = path + node.val.toString();
        
        // If node is a leaf, add path to results
        if (!node.left && !node.right) {
            paths.push(newPath);
        } else {
            // Push right and left children with the updated path
            if (node.right) stack.push([node.right, newPath + '->']);
            if (node.left) stack.push([node.left, newPath + '->']);
        }
    }

    return paths;
}
```

**Time Complexity:** \(O(N)\), where \(N\) is the number of nodes, as each node is visited once.

**Space Complexity:** \(O(N)\), because in the worst case, the stack contains all nodes of the tree.

---

Both solutions effectively provide all root-to-leaf paths in the binary tree, leveraging the DFS approach either recursively or iteratively for traversal. The selection between them depends on whether recursion or iteration is preferred in the given context.

