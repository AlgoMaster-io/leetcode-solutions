## [Leetcode 103: Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

### Approaches:
1. [Breadth-First Search (BFS) with Two Queues](#approach-1-bfs-with-two-queues)
2. [Breadth-First Search (BFS) with a Deque](#approach-2-bfs-with-deque)

---

### Approach 1: BFS with Two Queues

#### Intuition:
The basic idea is to perform a level-order traversal of the binary tree using two queues. We alternate the direction of inserting elements to mimic the zigzag pattern required by the problem.

#### Steps:
1. **Initialization**: Start with a queue initialized with the root node. Use a variable to track the current direction (left-to-right or right-to-left). Initialize a results array to store the final zigzag level order traversal.
2. **Iterative Traversal**: While there are nodes in the queue:
   - Track the number of nodes at the current level.
   - Depending on the direction, insert nodes in the level-order traversal either from left-to-right or right-to-left into a temporary array.
   - Enqueue children of each node for further processing in the next level.
   - Toggle the direction for the next level.
3. **Toggle and Store Result**: After processing each level, toggle the direction and store the temporary array into the results array. 

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = val ?? 0;
        this.left = left ?? null;
        this.right = right ?? null;
    }
}

function zigzagLevelOrder(root: TreeNode | null): number[][] {
    if (!root) return [];
    
    // Queue for BFS and results storage
    const queue: TreeNode[] = [root];
    const results: number[][] = [];
    let leftToRight = true; // to toggle direction

    // While there are nodes to process
    while (queue.length) {
        const levelSize = queue.length;
        const level: number[] = [];
        
        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift(); // dequeue front node
            if (!node) continue;

            // Depending on the direction, add the value to the level
            if (leftToRight) {
                level.push(node.val);
            } else {
                level.unshift(node.val);
            }
            
            // Enqueue left and right children for next level
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        
        // Push current level to results and toggle direction
        results.push(level);
        leftToRight = !leftToRight;
    }
    
    return results;
}
```

- **Time Complexity**: O(N) where N is the number of nodes in the tree (each node is processed once).
- **Space Complexity**: O(N) to store all nodes at a level (worst case could be half the number of total nodes).

---

### Approach 2: BFS with Deque

#### Intuition:
By using a deque (double-ended queue), the insertion of elements at either end of the level list becomes more efficient. This allows us to cleanly handle the zigzag pattern in a single data structure instead of toggling between arrays.

#### Steps:
1. **Initialize Deque**: Start with the root in a deque. Similar to the previous approach, use a toggle variable to manage directions.
2. **Iterative Traversal**: 
   - For each level, decide where insertions should begin (front or back of the deque).
   - Add children nodes to the deque for next level processing.
3. **Toggle and Store Results**: Toggle direction and append or prepend levels into the final result based on the toggle state.

```typescript
function zigzagLevelOrder(root: TreeNode | null): number[][] {
    if (!root) return [];
    
    const deque: TreeNode[] = [root];
    const results: number[][] = [];
    let leftToRight = true;

    while (deque.length) {
        const levelSize = deque.length;
        const level: number[] = [];
        
        for (let i = 0; i < levelSize; i++) {
            const node = deque.shift(); // dequeue front node
            if (!node) continue;
            
            // Depending on the direction, insert at the right position
            if (leftToRight) {
                level.push(node.val);
            } else {
                level.unshift(node.val);
            }
            
            // Enqueue children nodes for next level
            if (node.left) deque.push(node.left);
            if (node.right) deque.push(node.right);
        }
        
        results.push(level);
        leftToRight = !leftToRight; // toggle direction after each level
    }
    
    return results;
}
```

- **Time Complexity**: O(N) where N is the number of nodes in the tree.
- **Space Complexity**: O(N), where N can be the maximum width of the tree in worst case scenarios.

