# [Leetcode 117: Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

## Approaches:
1. [Level Order Traversal with Queue](#approach-1-level-order-traversal-with-queue)
2. [Using a Dummy Node for Constant Space](#approach-2-using-a-dummy-node-for-constant-space)

---

## Approach 1: Level Order Traversal with Queue

### Intuition
In this approach, we can perform a level-order traversal (breadth-first traversal) of the tree using a queue. By processing nodes level by level, we can link all the nodes within the same level. While traversing, we maintain a pointer to the previous node in the current level and connect each node's `next` pointer to this previous node. 

### Steps:
1. Use a queue to facilitate the level order traversal.
2. For each level, process every node and set the next pointer to the next node in the same level.
3. If a node has children, enqueue its children for the next level.
4. At the end of processing a level, make sure the last node's `next` pointer is `NULL`.

### Code
```javascript
function connect(root) {
    if (root === null) return null;
    
    let queue = [root];
    
    while (queue.length > 0) {
        let size = queue.length;
        let prev = null;
        
        for (let i = 0; i < size; i++) {
            let node = queue.shift();
            
            // Connect the previous node's next to the current node
            if (prev !== null) {
                prev.next = node;
            }
            prev = node;
            
            // Enqueue children for the next level
            if (node.left !== null) queue.push(node.left);
            if (node.right !== null) queue.push(node.right);
        }
        
        // Ensure the last node's next is set to NULL
        if (prev !== null) prev.next = null;
    }
    
    return root;
}
```

### Complexity Analysis
- **Time Complexity:** O(N) because each node is processed once.
- **Space Complexity:** O(W), where W is the maximum width of the tree (in the worst case, it can be the number of leaf nodes).

---

## Approach 2: Using a Dummy Node for Constant Space

### Intuition
This approach reduces the space complexity by using constant extra space. We use a dummy node to track the start of each level. As we connect nodes on the current level, we use this dummy node to facilitate moving to the next level.

### Steps:
1. Use a dummy node to manage the level transitions by connecting all nodes in the current level.
2. Iterate through each level using the current node of that level to connect the next level.
3. Connect the left and right children of each node using the dummy node.
4. Move to the next level using the dummy node's next pointer and repeat steps until no levels remain.

### Code
```javascript
function connect(root) {
    if (root === null) return null;
    
    let dummy = new Node(0); // a placeholder for level traversal
    let cur = root;
    
    while (cur !== null) {
        let currentLevel = dummy; // start from the dummy node at each level
        
        while (cur !== null) {
            // Connect the left child if it exists
            if (cur.left !== null) {
                currentLevel.next = cur.left;
                currentLevel = currentLevel.next;
            }
            // Connect the right child if it exists
            if (cur.right !== null) {
                currentLevel.next = cur.right;
                currentLevel = currentLevel.next;
            }
            // Move to the next node in the current level
            cur = cur.next;
        }
        
        // Move to the first node of the next level
        cur = dummy.next;
        dummy.next = null; // Reset the dummy for the next iteration
    }
    
    return root;
}
```

### Complexity Analysis
- **Time Complexity:** O(N) because each node is processed once.
- **Space Complexity:** O(1) because no extra space is used apart from pointers.

This approach is optimal for space usage thanks to the use of a dummy node.

