# Leetcode Problem 1110: Delete Nodes And Return Forest

You can view the problem statement [here](https://leetcode.com/problems/delete-nodes-and-return-forest/).

## Approaches
1. [Recursive Depth-First Search (DFS)](#recursive-depth-first-search-dfs)
2. [Iterative Depth-First Search using Stack](#iterative-depth-first-search-using-stack)

### 1. Recursive Depth-First Search (DFS)

**Intuition**:  
The problem asks us to delete certain nodes from a binary tree and return the forest that results, which is simply the collection of remaining trees. Using a recursive DFS approach allows us to process each node and decide its fate based on whether it needs to be deleted or kept. We'll systematically traverse each node, determine if it should be deleted, and manage its children accordingly.

**Approach**:  
- We recursively traverse each node of the binary tree.
- If a node is marked for deletion:
  - Add its non-null children to the forest since they will be the start of new trees.
- If a node is not marked for deletion:
  - It remains part of its current tree.
- After processing, return the root of the current tree if it’s not marked for deletion, otherwise null.

**Code**:
```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = val === undefined ? 0 : val;
        this.left = left === undefined ? null : left;
        this.right = right === undefined ? null : right;
    }
}

function delNodes(root: TreeNode | null, to_delete: number[]): TreeNode[] {
    const result: TreeNode[] = [];
    const deleteSet = new Set(to_delete);

    const helper = (node: TreeNode | null, isRoot: boolean): TreeNode | null => {
        if (!node) return null;

        // Check if this node needs to be deleted
        const deleted = deleteSet.has(node.val);

        // If this is root and not deleted, add to the result
        if (isRoot && !deleted) {
            result.push(node);
        }

        // Recur for left and right children
        node.left = helper(node.left, deleted);
        node.right = helper(node.right, deleted);

        // Return null if node was deleted
        return deleted ? null : node;
    };

    helper(root, true);
    return result;
}
```

**Time Complexity**: O(N), where N is the number of nodes in the tree, since every node is visited once.  
**Space Complexity**: O(N), considering the space used by the recursion stack in the worst case.

---

### 2. Iterative Depth-First Search using Stack

**Intuition**:  
An iterative DFS approach uses a stack to simulate the recursive stack. The main advantage is controlling the execution more explicitly, which might be beneficial to avoid stack overflow on very deep trees.

**Approach**:  
- Use a stack to control the traversal of the binary tree.
- For each node, determine if it needs to be deleted using the set of nodes to delete.
- If a node is deleted, its children could become new roots, so add them to the result list if they are not null.
- Maintain the parent to child linkage similarly as in the recursive approach.

**Code**:
```typescript
function delNodesIterative(root: TreeNode | null, to_delete: number[]): TreeNode[] {
    if (!root) return [];

    const result: TreeNode[] = [];
    const deleteSet = new Set(to_delete);
    const stack: [TreeNode, boolean][] = [[root, true]];

    while (stack.length) {
        const [node, isRoot] = stack.pop() as [TreeNode, boolean];
        const deleted = deleteSet.has(node.val);

        if (isRoot && !deleted) {
            result.push(node);
        }

        if (node.right) {
            stack.push([node.right, deleted]);
            if (deleted) node.right = null;
        }

        if (node.left) {
            stack.push([node.left, deleted]);
            if (deleted) node.left = null;
        }
    }

    return result;
}
```

**Time Complexity**: O(N), where N is the number of nodes in the tree, as each node is processed once.  
**Space Complexity**: O(N), due to the space used by the stack.

