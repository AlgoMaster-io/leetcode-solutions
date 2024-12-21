## [Leetcode 430: Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)

### Approaches
1. [Recursive Depth-First Search (DFS)](#recursive-dfs)
2. [Iterative Using a Stack](#iterative-stack)

---

### 1. Recursive Depth-First Search (DFS)

#### Intuition
The multilevel doubly linked list can be considered similar to a binary tree where each node has a child pointer representing the left subtree. Our task is to flatten this structure such that all nodes appear in a single level, like a doubly linked list. A recursive approach can effectively traverse each node and its children, appending them to the flattened list at the appropriate position.

- **Process**:
  - Start traversing from the head node.
  - For a node with a child, flatten the child level into a doubly linked list.
  - Connect the current node to the flattened child list, and then update the next pointers accordingly.

```python
class Node:
    def __init__(self, val, prev=None, next=None, child=None):
        self.val = val
        self.prev = prev
        self.next = next
        self.child = child
        
def flatten_dfs(node: 'Node') -> 'Node':
    if not node:
        return node
    
    # Helper function to recursively flatten the list and return the tail node
    def flatten_tail(curr: 'Node') -> 'Node':
        head = curr
        tail = None

        while curr:
            next_node = curr.next
            if curr.child:
                # Recursively flatten the child and get the tail of the child list
                child_tail = flatten_tail(curr.child)

                # Connect the current node to the child head
                curr.next = curr.child
                curr.child.prev = curr

                # Connect the child tail to the next_node
                child_tail.next = next_node
                if next_node:
                    next_node.prev = child_tail
                
                # Nullify the child pointer
                curr.child = None 

                # Update the current node to the child's tail
                curr = child_tail
            else:
                # If there's no child, just move to the next node
                curr = next_node
            
            # In all cases, update the tail to the current node
            if curr:
                tail = curr

        return tail
    
    flatten_tail(node)
    return node
```

- **Time Complexity**: O(n), where n is the total number of nodes in the list, since every node is visited once.
- **Space Complexity**: O(1) for extra space usage, although recursion uses stack space up to O(d), where d is the maximum depth of recursion.

---

### 2. Iterative Using a Stack

#### Intuition
Using an iterative approach with a stack provides a non-recursive method to flatten the list. By simulating the recursive call stack iteratively, we can ensure compatibility with deeper nested structures that could otherwise lead to stack overflow in a purely recursive solution.

- **Process**:
  - Utilize a stack to keep track of the node's next reference whenever a child exists.
  - Traverse the current level and flatten each level onto the result while managing child nodes through the stack.
  - This method avoids deep recursion by keeping stacking manageable.

```python
def flatten_iterative(head: 'Node') -> 'Node':
    if not head:
        return head
    
    stack = []
    curr = head
    
    while curr or stack:
        # If the current node has a child
        if curr.child:
            # If the current node has a 'next', we will need to revisit it later
            if curr.next:
                stack.append(curr.next)
            
            # Point the current 'next' to the child, and update child's prev to current
            curr.next = curr.child
            curr.next.prev = curr
            curr.child = None  # Remove the child link
        
        # If we reach the end of the current list level and we have nodes in the stack
        if not curr.next and stack:
            # Pop from the stack and assign as current's next
            curr.next = stack.pop()
            curr.next.prev = curr
        
        # Move to the next node
        curr = curr.next
    
    return head
```

- **Time Complexity**: O(n), where n is the total number of nodes in the list, since every node is visited once.
- **Space Complexity**: O(m), where m is the maximum number of multilevel nodes we might stack, dictated by the maximum number of nodes at any point in recursion.

