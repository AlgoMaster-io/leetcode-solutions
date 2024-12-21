# [Leetcode 222: Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

## Approaches
- [Approach 1: Recursive Count (Naive)](#approach-1-recursive-count)
- [Approach 2: Iterative Count (Improved)](#approach-2-iterative-count)
- [Approach 3: Binary Search on Tree Level (Optimal)](#approach-3-binary-search-on-tree-level)


### Approach 1: Recursive Count

#### Intuition:
The simplest way to count the nodes in a complete binary tree is to traverse it and count every node we visit. This can be achieved using a straightforward recursive method where we visit each node once.

#### Code:
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

function countNodes(root: TreeNode | null): number {
    if (root === null) return 0;

    // Recursively count nodes in left and right subtree and add 1 for the root.
    return 1 + countNodes(root.left) + countNodes(root.right);
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of nodes in the tree. Each node is visited once.
- **Space Complexity**: O(h), where h is the height of the tree. This accounts for the call stack during recursion.

### Approach 2: Iterative Count

#### Intuition:
An iterative version of the recursive approach can help manage large tree sizes better by avoiding large recursion stacks. We can use a queue for breadth-first traversal, counting nodes as we visit them.

#### Code:
```typescript
function countNodesIterative(root: TreeNode | null): number {
    if (root === null) return 0;
    let count = 0;
    const queue: TreeNode[] = [root];

    while (queue.length > 0) {
        const node = queue.shift()!; // Remove head of queue
        count += 1; // Count the current node

        // Add children to the queue
        if (node.left !== null) queue.push(node.left);
        if (node.right !== null) queue.push(node.right);
    }
    return count;
}
```

#### Complexity Analysis:
- **Time Complexity**: O(n), where n is the number of nodes in the tree.
- **Space Complexity**: O(n) in the worst case for storing nodes in the queue.

### Approach 3: Binary Search on Tree Level

#### Intuition:
In a complete binary tree, except possibly the last level, all levels are completely filled, and nodes are filled from left to right. This property allows for a more efficient counting strategy using binary search. The idea is to utilize the tree's depth to determine the number of nodes in the last level and add it to the count of nodes in all fully filled levels.

#### Code:
```typescript
function countNodesOptimal(root: TreeNode | null): number {
    if (root === null) return 0;

    // Get the depth of leftmost path (which is the total number of levels)
    let depth = 0;
    let node = root;
    while (node.left !== null) {
        depth++;
        node = node.left;
    }

    // Function to check if a node exists at the given index in the last level
    const exists = (index: number, depth: number, node: TreeNode): boolean => {
        let left = 0, right = (1 << depth) - 1;
        for (let d = 0; d < depth; d++) {
            let pivot = Math.floor((left + right) / 2);
            if (index <= pivot) {
                node = node.left!;
                right = pivot;
            } else {
                node = node.right!;
                left = pivot + 1;
            }
        }
        return node !== null;
    }

    let left = 0, right = (1 << depth) - 1;
    while (left <= right) {
        let pivot = Math.floor((left + right) / 2);
        if (exists(pivot, depth, root)) {
            left = pivot + 1;
        } else {
            right = pivot - 1;
        }
    }

    // The total number of nodes is all nodes in the complete levels plus the number of nodes in the last level.
    return (1 << depth) - 1 + left;
}
```

#### Complexity Analysis:
- **Time Complexity**: O((log n)^2), where n is the number of nodes. We do a binary search (log n) and each search involves a depth-level traversal (another log n).
- **Space Complexity**: O(1) since no additional data structures need considerable space besides a few variables.

