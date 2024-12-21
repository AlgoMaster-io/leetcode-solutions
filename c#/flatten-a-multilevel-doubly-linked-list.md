# [Leetcode 430: Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)

## Approaches:
- [Approach 1: Recursive DFS](#approach-1-recursive-dfs)
- [Approach 2: Iterative with Stack](#approach-2-iterative-with-stack)

### Approach 1: Recursive DFS

The recursive approach employs a Depth First Search (DFS) strategy to flatten the multilevel doubly linked list. The key intuition here is to process each node recursively, flattening the child nodes (if present) before connecting it back to the main list. 

1. Begin the recursion from the head node.
2. For each node, check if it has a child:
   - If a child exists, recursively flatten the child list.
   - Connect the flattened child list between the current node and its next node.
3. Return the new resultant list starting from the head.

#### Code:
```csharp
/*
// Definition for a Node.
public class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
}

public class Solution {
    public Node Flatten(Node head) {
        if (head == null) return null;
        
        // Helper function to perform DFS and flatten the list
        FlattenDFS(head);
        return head;
    }
    
    private Node FlattenDFS(Node node) {
        var current = node;
        Node last = null;
        
        while (current != null) {
            Node next = current.next; // Store the next node
            
            if (current.child != null) {
                // Flatten the child first
                Node childLast = FlattenDFS(current.child);

                // Connect current, child, and next nodes
                current.next = current.child;
                current.child.prev = current;

                // If there's a next node, connect the it to the end of the child list
                if (next != null) {
                    childLast.next = next;
                    next.prev = childLast;
                }
                
                // Set the child pointer to null, as it's now part of the main list
                current.child = null;
                
                last = childLast;
            } else {
              last = current;  
            }
            
            current = next; // Move to the next node (original if no child, or new connected one)
        }
        
        return last; // Return the last node we processed
    }
}
```

#### Complexity:
- **Time Complexity**: O(N), where N is the total number of nodes in the multilevel list. Each node is visited once.
- **Space Complexity**: O(N) in the worst case due to the recursion stack for depth-first traversal.

### Approach 2: Iterative with Stack

This approach utilizes a stack to simulate the recursive call stack in an iterative manner. The intuition is similar: for each node, process the child nodes first by pushing next nodes to the stack, thereby flattening recursively.

1. Use a stack to keep track of nodes.
2. Start with the head node, and process each node's child, flattening it onto the next.
3. Whenever a node with a child is encountered, push its next node to the stack and process its child.
4. Continue until the entire list is flattened.

#### Code:
```csharp
public class Solution {
    public Node Flatten(Node head) {
        if (head == null) return null;
        
        Stack<Node> stack = new Stack<Node>();
        Node current = head;

        while (current != null) {
            if (current.child != null) {
                if (current.next != null) {
                    stack.Push(current.next); // Save the next node
                }
                
                // Make child the next node and adjust pointers
                current.next = current.child;
                current.child.prev = current;
                current.child = null;
            }

            if (current.next == null && stack.Count > 0) {
                current.next = stack.Pop(); // Reconnect the next to the previously saved node
                current.next.prev = current;
            }
            
            current = current.next;
        }
        
        return head;
    }
}
```

#### Complexity:
- **Time Complexity**: O(N), where N is the number of nodes in the multilevel list.
- **Space Complexity**: O(N) in the worst case caused by the stack, which could end up holding all the next nodes temporarily.

