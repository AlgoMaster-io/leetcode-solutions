# [Leetcode 102: Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## Approaches
- [Breadth-First Search (BFS) using Queue](#bfs-using-queue)
- [Recursive Level Order Traversal](#recursive-level-order-traversal)

---

## BFS using Queue

The most intuitive way to perform a level order traversal is by using a queue. Here's how the approach can be broken down:

1. **Intuition:** 
   - A queue is a natural fit for breadth-first search because it follows the FIFO (first-in, first-out) principle, which aligns well with processing nodes level by level.
   - We can enqueue the root node and then continuously dequeue nodes from the front of the queue, while enqueuing their children at the back.
   - For each node processed, check its level and append it to the results.

2. **Steps:** 
   - Initialize a queue and push the root node, along with an indication of its level (level 0).
   - Dequeue a node. If the queue is empty after the dequeue, we've processed all nodes.
   - Append the node's value to the appropriate level in the result list.
   - Enqueue the node's left and right children if they exist, each with an incremented level.

3. **Code Implementation**

```typescript
class TreeNode {
    val: number
    left: TreeNode | null
    right: TreeNode | null
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val)
        this.left = (left===undefined ? null : left)
        this.right = (right===undefined ? null : right)
    }
}

function levelOrder(root: TreeNode | null): number[][] {
    // Return empty if root is null
    if (!root) return [];
    
    const result: number[][] = [];
    const queue: {node: TreeNode, level: number}[] = [{node: root, level: 0}];
    
    while (queue.length > 0) {
        const {node, level} = queue.shift()!;
        
        // Check if the level exists in the result
        if (result.length === level) {
            result.push([]);
        }
        
        // Add node value to the corresponding level in result
        result[level].push(node.val);
        
        // Enqueue children nodes with the next level
        if (node.left) {
            queue.push({node: node.left, level: level + 1});
        }
        if (node.right) {
            queue.push({node: node.right, level: level + 1});
        }
    }
    
    return result;
}
```

- **Time Complexity:** O(N) where N is the number of nodes, as each node is visited once.
- **Space Complexity:** O(N) for the queue and the result storing all nodes.

---

## Recursive Level Order Traversal

An alternative method is to utilize recursion to achieve the same level order traversal:

1. **Intuition:** 
   - Utilize a helper function to track the level/depth we're currently processing.
   - Instead of using a queue data structure, recursive function calls will help navigate the tree.
   - Base the function on the depth of the node and ensure it contributes to the correct level.

2. **Steps:** 
   - Define a recursive helper function that takes a node, the current level, and the results array.
   - If visiting a node at a new level, add a new array for that level in the result.
   - Append the current node's value to its corresponding level.
   - Call the helper function recursively for the left and right children, increasing the level.

3. **Code Implementation**

```typescript
function levelOrderRecursive(root: TreeNode | null): number[][] {
    const result: number[][] = [];
    
    function traverse(node: TreeNode | null, level: number) {
        if (!node) return;
        
        // Add new array for a new level
        if (result.length === level) {
            result.push([]);
        }
        
        // Append current node's value to the correct level
        result[level].push(node.val);
        
        // Recurse left and right children with incremented level
        traverse(node.left, level + 1);
        traverse(node.right, level + 1);
    }
    
    // Initiate recursion at level 0
    traverse(root, 0);
    
    return result;
}
```

- **Time Complexity:** O(N) since each node is visited once.
- **Space Complexity:** O(N) for the implicit call stack and the final result, storing all nodes.

