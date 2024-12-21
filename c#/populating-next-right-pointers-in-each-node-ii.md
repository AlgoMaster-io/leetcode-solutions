# [Leetcode Problem 117: Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

## Table of Contents
- [Approach 1: Level Order Traversal (BFS) Using a Queue](#approach-1-level-order-traversal-bfs-using-a-queue)
- [Approach 2: Using Previously Established Next Pointers](#approach-2-using-previously-established-next-pointers)

---

## Approach 1: Level Order Traversal (BFS) Using a Queue

### Intuition
The idea here is to traverse the tree level by level using a queue (similar to BFS). As we traverse each level, we will connect each node to its next right node. This is a straightforward approach that uses extra space for the queue to hold nodes of each level.

### Implementation
```csharp
public Node Connect(Node root) {
    if (root == null) return null;

    // Queue to facilitate level order traversal
    Queue<Node> queue = new Queue<Node>();
    queue.Enqueue(root);

    while (queue.Count > 0) {
        int size = queue.Count;
        Node prev = null;

        // For all nodes at the current level
        for (int i = 0; i < size; i++) {
            Node node = queue.Dequeue();

            // Connect the previous node to the current node
            if (prev != null) {
                prev.next = node;
            }
            prev = node;

            // Add children of current node to the queue for the next level
            if (node.left != null) queue.Enqueue(node.left);
            if (node.right != null) queue.Enqueue(node.right);
        }
        
        // Last node of the level should point to null, handled by default
        prev.next = null;
    }

    return root;
}
```

### Time and Space Complexity
- **Time Complexity:** O(n), where n is the number of nodes in the tree, because each node is processed exactly once.
- **Space Complexity:** O(n), in the worst case scenario, where the queue will hold all nodes at the largest level in the tree.

---

## Approach 2: Using Previously Established Next Pointers

### Intuition
Instead of using additional space with a queue, we can utilize the already set `next` pointers to traverse each level. In this approach, we establish the next level pointers using the next pointers that have already been set for the current level.

### Implementation
```csharp
public Node Connect(Node root) {
    Node current = root;  // Start with the root
    while (current != null) {
        // Dummy node to handle the next level
        Node dummyHead = new Node(0);
        Node prev = dummyHead;

        // Iterate nodes in the current level
        while (current != null) {
            if (current.left != null) {
                prev.next = current.left;
                prev = prev.next;
            }
            if (current.right != null) {
                prev.next = current.right;
                prev = prev.next;
            }
            // Move to the next node in the current level
            current = current.next;
        }

        // Move to the next level
        current = dummyHead.next;
    }

    return root;
}
```

### Time and Space Complexity
- **Time Complexity:** O(n), because each node is visited exactly once.
- **Space Complexity:** O(1), as no additional space proportional to the input size is used.

---

These approaches illustrate different strategies to connect the next right pointers in a binary tree, with the second approach being more space efficient, leveraging already established connections within the tree.

