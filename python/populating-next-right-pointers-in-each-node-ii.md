# [Leetcode 117: Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

## Approaches
- [Approach 1: Level Order Traversal Using Queue](#approach-1-level-order-traversal-using-queue)
- [Approach 2: Using Pointers without Extra Space](#approach-2-using-pointers-without-extra-space)

### Approach 1: Level Order Traversal Using Queue

**Intuition:**

The first intuitive solution is using a level order traversal which involves iterating over each level of the tree. By leveraging a queue, we can control the process of visiting nodes by level, allowing us to efficiently set the `next` pointers whilst traversing.

**Steps:**

1. Utilize a queue to store the nodes level by level.
2. Start by enqueueing the root node.
3. For each iteration, process each node's children by checking existing nodes at the current level:
   - Set the `next` pointer of each node to the subsequent node in the queue (i.e., to the right or None if it is the last node at its level).
4. Enqueue the children of the outer nodes to handle further levels.
5. Iterate until all nodes are processed.

**Time Complexity:** O(N), where N is the number of nodes. Each node is processed once.
**Space Complexity:** O(N), as at most, half of the nodes (nodes at the bottom level) could be in the queue simultaneously.

```python
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next

from collections import deque

def connect(root: 'Node') -> 'Node':
    if not root:
        return None

    queue = deque([root])

    while queue:
        size = len(queue)
        for i in range(size):
            node = queue.popleft()
            # Connect the node with the next node in the queue
            if i < size - 1:
                node.next = queue[0]
            # Add children to the queue for the next level
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return root
```

### Approach 2: Using Pointers without Extra Space

**Intuition:**

By eliminating use of extra space, we can still achieve the objective by using dummy nodes and pointers. The basic idea is to use a "dummy" node to facilitate moving through the nodes at the next level while connecting them via `next` pointers. This also means utilizing previously connected `next` pointers to move to the next nodes at the current level.

**Steps:**

1. Use a dummy node to represent the head of nodes at the next level.
2. Maintain a `current` pointer to traverse nodes at the current level.
3. For each node at the current level, link its children using the `next` pointer.
4. Move node by node at the current level, setting their children `next` pointers.

**Time Complexity:** O(N), as all nodes are visited exactly once.
**Space Complexity:** O(1), no extra space beyond variables used, as connections are made in place.

```python
def connect(root: 'Node') -> 'Node':
    if not root:
        return None

    # Initialize the first level
    current = root
    while current:
        dummy = Node(0)  # Sentinel node
        tail = dummy  # Tail node to keep track of next level's connection

        # Traverse current level
        while current:
            if current.left:
                tail.next = current.left
                tail = tail.next  # Move tail

            if current.right:
                tail.next = current.right
                tail = tail.next  # Move tail

            # Move to next node in the current level
            current = current.next

        # Move to the next level
        current = dummy.next
    
    return root
```

Each of the provided approaches sequentially increases in optimization, from using a queue to using pointers that eliminate extra space usage, offering flexibility according to constraints or requirements specific to practical implementations.

