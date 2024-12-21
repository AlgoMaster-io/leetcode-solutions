# [Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

## Approaches
- [Approach 1: Breadth-First Search (Level Order Traversal)](#approach-1-breadth-first-search-level-order-traversal)
- [Approach 2: Using O(1) Space with a Dummy Node](#approach-2-using-o1-space-with-a-dummy-node)

### Approach 1: Breadth-First Search (Level Order Traversal)

**Intuition:**

To solve this problem, we can perform a level-order traversal using a queue. This approach is somewhat similar to the basic breadth-first search algorithm where nodes are traversed horizontally level by level.

For each level of the tree, we link each node to its next right neighbor using a queue to maintain the nodes of the current level. After processing each level, the queue contains nodes of the next level.

**Steps:**
1. If the root is null, simply return nil.
2. Initialize a queue and add the root node to it.
3. While the queue is not empty:
   - Determine the number of nodes at the current level (size of the queue).
   - Iterate over each node at the current level.
   - Set the `next` pointer of the current node to the next node in the queue.
   - Add the non-null left and right children of the current node to the queue.
4. Continue this process until the queue is empty.

**Code:**

```go
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Left *Node
 *     Right *Node
 *     Next *Node
 * }
 */
func connect(root *Node) *Node {
    if root == nil {
        return nil
    }
    
    queue := []*Node{root}

    for len(queue) > 0 {
        size := len(queue)
        for i := 0; i < size; i++ {
            node := queue[0]
            queue = queue[1:]
            // Connect the current node's next to the subsequent node in the queue
            if i < size - 1 {
                node.Next = queue[0]
            }

            // Add the left and right children to the queue if they exist
            if node.Left != nil {
                queue = append(queue, node.Left)
            }
            if node.Right != nil {
                queue = append(queue, node.Right)
            }
        }
    }
    
    return root
}
```

**Complexity Analysis:**
- **Time Complexity:** O(n), where n is the number of nodes in the tree. Each node is visited once.
- **Space Complexity:** O(n) in the worst case due to the queue, particularly when the tree is completely unbalanced.

### Approach 2: Using O(1) Space with a Dummy Node

**Intuition:**

To optimize space usage, we can use a dummy node to construct a pseudo-level of the next level. Instead of maintaining a queue, we use pointers to connect nodes level by level without additional space for traversal.

Here's how it works:
- We keep a `current` pointer starting at the root to track nodes at the current level we are processing.
- We use a `dummy` node to help build the `next` level's linked list of nodes.
- We iterate through the `current` level, connecting each node's children using the `dummy` node.

**Steps:**
1. Create a dummy node for facilitating pointer connections between nodes of the next level.
2. Initialize the `current` pointer to the root.
3. For each current node, connect its children using a `level` traversal through its `Next` pointers.
4. Once the current level is processed, move to the next level using the connections made.
5. Repeat the process until there are no more levels to process.

**Code:**

```go
func connect(root *Node) *Node {
    if root == nil {
        return nil
    }
    
    current := root
    for current != nil {
        dummy := &Node{}
        tail := dummy
        
        for node := current; node != nil; node = node.Next {
            if node.Left != nil {
                tail.Next = node.Left
                tail = tail.Next
            }
            if node.Right != nil {
                tail.Next = node.Right
                tail = tail.Next
            }
        }
        
        current = dummy.Next
    }
    
    return root
}
```

**Complexity Analysis:**
- **Time Complexity:** O(n), since every node is visited once.
- **Space Complexity:** O(1), as only a few extra pointers (dummy, tail) are used regardless of input size.

