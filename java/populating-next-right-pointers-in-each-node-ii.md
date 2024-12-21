# [Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

## Approaches
1. [Level Order Traversal using Queue](#approach-1-level-order-traversal-using-queue)
2. [Using Previously Established Next Pointers](#approach-2-using-previously-established-next-pointers)

### Approach 1: Level Order Traversal using Queue

#### Intuition
The basic idea is to use a queue to perform a level order traversal of the tree. For each node at a particular level, connect it to its next right node using the queue. This method ensures that we connect all nodes at the same level before moving to the next level.

1. Start by enqueuing the root node.
2. Iterate while the queue is not empty. For every iteration, it represents a new level.
3. For each node at the current level, link node's `next` to the next node in the queue.
4. Enqueue the left and right children of the node if they exist.
5. Repeat the above steps until all levels are processed.

#### Code
```java
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node next;

    public Node() {}
    
    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _left, Node _right, Node _next) {
        val = _val;
        left = _left;
        right = _right;
        next = _next;
    }
}

class Solution {
    public Node connect(Node root) {
        if (root == null) return null;
        
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            int size = queue.size();
            Node prev = null;

            for (int i = 0; i < size; i++) {
                Node currentNode = queue.poll();

                if (prev != null) {
                    prev.next = currentNode;
                }
                
                prev = currentNode;

                if (currentNode.left != null) {
                    queue.add(currentNode.left);
                }

                if (currentNode.right != null) {
                    queue.add(currentNode.right);
                }
            }
        }
        return root;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** O(N), where N is the total number of nodes. Each node is processed once.
- **Space Complexity:** O(N), the space used by the queue in the worst case.

### Approach 2: Using Previously Established Next Pointers

#### Intuition
To optimize space, we can avoid using a queue and instead leverage the `next` pointers established during the traversal. Instead of processing nodes level by level, the idea is to build the next pointers for the next level while traversing the current level.

1. Use a current pointer to traverse nodes at the current level.
2. Use a dummy node to represent the start of the next level.
3. Track the last visited node in the next level using a tail pointer starting from the dummy node.
4. Move to the next level by setting the current pointer to `dummy.next` once the current level is completely processed.

#### Code
```java
class Solution {
    public Node connect(Node root) {
        Node dummyHead = new Node(0);
        Node current = root;

        while (current != null) {
            Node tail = dummyHead;  // Tail for the next level, starting from the dummy head.
            
            // Iterate over nodes in the current level
            while (current != null) {
                if (current.left != null) {
                    tail.next = current.left;
                    tail = tail.next;
                }
                
                if (current.right != null) {
                    tail.next = current.right;
                    tail = tail.next;
                }
                
                current = current.next;
            }

            // Move to the start of the next level
            current = dummyHead.next;
            dummyHead.next = null;  // Reset dummy head's next for the next iteration/level.
        }

        return root;
    }
}
```

#### Complexity Analysis
- **Time Complexity:** O(N), where N is the total number of nodes. Each node is processed once.
- **Space Complexity:** O(1), as we're only using a few additional pointers, not a queue.

The second approach is more space-efficient, leveraging the established next pointers to traverse the tree by levels, thus avoiding the additional queue storage space.

