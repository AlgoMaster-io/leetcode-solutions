# [Leetcode 222: Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

## Approaches
1. [Brute Force - Level Order Traversal](#solution-1)
2. [Recursive Count](#solution-2)
3. [Binary Search on Tree Depth](#solution-3)

---

## Solution 1: Brute Force - Level Order Traversal

### Intuition
A complete binary tree is a binary tree where each level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. A straightforward approach to count the nodes of such a tree is to perform a level order traversal and count each node encountered.

### Approach
- Use a queue to perform a level order traversal (BFS) of the tree.
- Initialize a count to zero.
- Traverse each node, incrementing the count each time a node is dequeued.
- This will give us the total number of nodes in the tree.

```javascript
function countNodes(root) {
    if (!root) return 0;

    let count = 0;
    const queue = [root];

    while (queue.length > 0) {
        const node = queue.shift(); // Dequeue node
        count++; // Count the current node

        // If the left child exists, enqueue it
        if (node.left) queue.push(node.left);

        // If the right child exists, enqueue it
        if (node.right) queue.push(node.right);
    }

    return count;
}
```

- **Time Complexity:** O(n), where n is the total number of nodes in the tree.
- **Space Complexity:** O(n) for the queue used in level order traversal.

---

## Solution 2: Recursive Count

### Intuition
We can leverage the definition of complete binary trees to count nodes recursively. If a tree is complete, except the last level, it means we can split our problem into counting nodes in the left and right subtrees.

### Approach
- Recursively visit left and right subtrees and count nodes.
- The base case is when the node is null, in which case the count is 0.
- Otherwise, the count for the node is 1 (itself) plus the count of nodes in its left and right subtrees.

```javascript
function countNodes(root) {
    if (!root) return 0;
    
    // Count nodes of left and right subtree and add 1 for the root node
    return 1 + countNodes(root.left) + countNodes(root.right);
}
```

- **Time Complexity:** O(n), where n is the total number of nodes in the tree.
- **Space Complexity:** O(h), where h is the height of the tree, due to recursive stack space usage.

---

## Solution 3: Binary Search on Tree Depth

### Intuition
For a complete binary tree, all levels before the last are fully filled, which means we can leverage properties of the tree structure to count nodes more efficiently using binary search on the tree depth.

### Approach
- First, calculate the depth of the leftmost path in the tree to determine the total levels.
- Perform a binary search on the last level to determine how many nodes exist. This is done by checking paths to the left and right children:
  - The idea is to check whether mid node exists by navigating from the root.
- Use bitwise operations to navigate through the tree, avoiding the need to recreate paths from scratch for each node.

```javascript
function countNodes(root) {
    if (!root) return 0;

    let depth = getDepth(root);

    if (depth === 0) return 1;

    let left = 1;
    let right = Math.pow(2, depth) - 1;

    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        if (nodeExists(mid, depth, root)) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    return Math.pow(2, depth) - 1 + left;
}

function getDepth(node) {
    let depth = 0;
    while (node.left) {
        depth++;
        node = node.left;
    }
    return depth;
}

function nodeExists(idx, depth, node) {
    let left = 0;
    let right = Math.pow(2, depth) - 1;
    for (let i = 0; i < depth; i++) {
        let mid = Math.floor((left + right) / 2);
        if (idx <= mid) {
            node = node.left;
            right = mid;
        } else {
            node = node.right;
            left = mid + 1;
        }
    }
    return node !== null;
}
```

- **Time Complexity:** O(log^2(n)), where n is the number of nodes. The depth calculation is O(log(n)), and checking each node existence is O(log(n)) for each depth.
- **Space Complexity:** O(1), neither stack nor additional data structures used, except the call stack which is not extensive.

---

Each of these solutions provides a way to count nodes in a complete binary tree, using different techniques and providing insight into the trade-offs between simplicity and efficiency.

