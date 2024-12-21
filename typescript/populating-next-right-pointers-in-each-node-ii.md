# [Leetcode 117: Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

## Approaches
- [Approach 1: Level Order Traversal using Queue](#approach-1-level-order-traversal-using-queue)
- [Approach 2: Optimized Level Order using Next Pointers](#approach-2-optimized-level-order-using-next-pointers)

### Approach 1: Level Order Traversal using Queue

**Intuition:**

The basic idea is to use a level order traversal (Breadth-First Search) using a queue. For each level, we need to connect the `next` pointers of each node to the adjacent node at the same level. By processing nodes level by level, we can ensure that all nodes are connected properly.

**Steps:**
1. Initialize a queue with the root node.
2. Process nodes level by level:
   - Keep track of the size of the current level.
   - Iterate over all nodes at this level, connect their `next` pointer to the next node in the queue.
   - Enqueue left and right children of the current node.
3. Continue until all levels are processed and all nodes are connected.

**Time Complexity:** \(O(N)\), where \(N\) is the number of nodes in the tree, since each node is processed once.

**Space Complexity:** \(O(W)\), where \(W\) is the maximum width of the tree (the most number of nodes at any level).

```typescript
class Node {
    val: number;
    left: Node | null;
    right: Node | null;
    next: Node | null;

    constructor(val: number, left: Node | null = null, right: Node | null = null, next: Node | null = null) {
        this.val = val;
        this.left = left;
        this.right = right;
        this.next = next;
    }
}

function connect(root: Node | null): Node | null {
    if (!root) {
        return null;
    }
    
    // Initialize queue with root
    const queue: Node[] = [root];
    
    while (queue.length > 0) {
        let size = queue.length;
        let previousNode: Node | null = null;
        
        for (let i = 0; i < size; i++) {
            const currentNode = queue.shift()!;
            
            // If there's a previous node, point it to the current node
            if (previousNode) {
                previousNode.next = currentNode;
            }
            
            // Update the previous node to the current one
            previousNode = currentNode;
            
            // Enqueue left and right children if they exist
            if (currentNode.left) {
                queue.push(currentNode.left);
            }
            if (currentNode.right) {
                queue.push(currentNode.right);
            }
        }
        // Last node in the level points to null implicitly
    }
    
    return root;
}
```

### Approach 2: Optimized Level Order using Next Pointers

**Intuition:**

Instead of using a queue, we can use the `next` pointers to traverse nodes at the current level. For each level, maintain a dummy node to build the `next` pointers for the level below it. After setting the `next` pointers for a complete level, move to the next level using the previously set `next` pointers.

**Steps:**
1. Start with the current node (initially the root).
2. Use a dummy node setup to connect `next` pointers for children of all nodes at the current level.
3. Traverse through current level using `next` pointers.
4. At each level, create the `next` connections for its children.
5. After finishing one level, move to the next using the dummy node's next pointer.

**Time Complexity:** \(O(N)\), where \(N\) is the number of nodes, since each node and each connection is handled only once.

**Space Complexity:** \(O(1)\), since we are using no additional data structures apart from pointers.

```typescript
class Node {
    val: number;
    left: Node | null;
    right: Node | null;
    next: Node | null;

    constructor(val: number, left: Node | null = null, right: Node | null = null, next: Node | null = null) {
        this.val = val;
        this.left = left;
        this.right = right;
        this.next = next;
    }
}

function connect(root: Node | null): Node | null {
    // Start with the root node
    let current = root;
    
    while (current) {
        // Dummy node for the next level
        let dummyHead = new Node(0);
        let tail = dummyHead;
        
        // Traverse nodes at the current level
        while (current) {
            if (current.left) {
                tail.next = current.left;
                tail = tail.next;
            }
            if (current.right) {
                tail.next = current.right;
                tail = tail.next;
            }
            
            // Move to the next node in the current level using the next pointer
            current = current.next;
        }
        
        // Move to the next level
        current = dummyHead.next;
    }
    
    return root;
}
```
This approach uses the structure of the given tree to minimize data usage and follows a pattern of creating a linked list connection for each next level using dummy nodes and the next pointers themselves, enhancing the efficiency by utilizing the already available pointers.

