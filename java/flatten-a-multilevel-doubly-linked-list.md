### [Leetcode Problem 430: Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)

---

#### Approaches

1. [Recursive Depth-First Search (DFS)](#recursive-depth-first-search-dfs)
2. [Iterative Using Stack](#iterative-using-stack)

---

### Recursive Depth-First Search (DFS)

#### Intuition
The multilevel doubly linked list can be visualized as a tree structure where some nodes have children. The task is equivalent to performing a depth-first traversal and flattening the tree into a single-level doubly linked list. 

The recursive approach involves:
- Traversing each node,
- Recursively flattening any child node, and
- Splicing the flattened child list into the main list.
  
#### Steps
1. Start from the head of the list.
2. Traverse the list. For each node, check if it has a child.
3. If a child exists, recursively flatten the child list.
4. Splice the child list between the current node and the rest of the main list.
5. Move the pointer to the next node and repeat the process.
   
#### Java Code

```java
// Node class definition provided by problem
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
}

public class Solution {
    public Node flatten(Node head) {
        flattenHelper(head);
        return head;
    }

    // Recursive helper that returns the last node of the flattened list
    private Node flattenHelper(Node node) {
        Node curr = node;
        Node last = node;

        while (curr != null) {
            Node next = curr.next;
            if (curr.child != null) {
                Node childLast = flattenHelper(curr.child);

                // Splice child list into main list
                curr.next = curr.child;
                curr.child.prev = curr;

                if (next != null) {
                    childLast.next = next;
                    next.prev = childLast;
                }
                
                curr.child = null;
                last = childLast;
            } else {
                last = curr;
            }
            curr = next;
        }
        
        return last;
    }
}
```

#### Time Complexity
- O(N), where N is the total number of nodes because each node is visited once.

#### Space Complexity
- O(N) in the worst case due to the recursive function call stack.

---

### Iterative Using Stack

#### Intuition
An iterative approach can be implemented using a stack to simulate the recursive flow. By using a stack, we can handle the nested and continuation structure of the list explicitly:

- Traverse through the list
- When encountering a node with a child, push the next node to a stack for later processing.
- Splice the child node into the main list.
  
#### Steps
1. Use a stack to keep track of the remaining nodes after encountering a child.
2. Iterate over the list.
3. If a node has a child, push its next to the stack, splice the child into the list, and continue.
4. After processing the child, continue with the node from the stack if available.
  
#### Java Code

```java
import java.util.Stack;

public class Solution {
    public Node flatten(Node head) {
        if (head == null) return head;

        Stack<Node> stack = new Stack<>();
        Node curr = head;

        while (curr != null) {
            if (curr.child != null) {
                if (curr.next != null) {
                    stack.push(curr.next);
                }

                // Connect the child list to the current node
                curr.next = curr.child;
                curr.child.prev = curr;
                curr.child = null; // Don't forget to clear the child pointer
            }

            if (curr.next == null && !stack.isEmpty()) {
                // Pop from stack and attach it to the end of the current list
                curr.next = stack.pop();
                curr.next.prev = curr;
            }

            curr = curr.next;
        }

        return head;
    }
}
```

#### Time Complexity
- O(N), where N is the total number of nodes processed.

#### Space Complexity
- O(N) due to the maximum size of the stack in the worst scenario. Each node is pushed onto the stack once when its child is processed.

