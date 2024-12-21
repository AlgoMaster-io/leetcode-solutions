# [Leetcode 430: Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)

## Approaches

1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach with Stack](#iterative-approach-with-stack)

---

## Recursive Approach

### Intuition

The idea behind the recursive approach is to traverse the multilevel doubly linked list in depth-first manner. Each node may have a child list, and the task is to flatten this hierarchy into a single-level doubly linked list. By using recursion, we can dive into each child node and append it to the current list, ensuring the original structure is preserved in a flattened form.

### Steps

1. Start from the head of the list. If it's null, return it immediately.
2. Traverse the list node by node.
3. If a node has a child, flatten the child list using recursion and append it between the current node and the next node.
4. Adjust the pointers: 
   - Set the current node's next to the head of the flattened child list.
   - Update the flattened child list's last node's next to the node that originally followed the current node.
   - Ensure the previous links are set correctly.
5. Move to the next node and repeat the process.
6. Continue until all nodes are processed.

### Time Complexity

- O(N) where N is the total number of nodes including all children. We visit each node exactly once.

### Space Complexity

- O(N) due to the recursion stack.

### Code

```javascript
function flatten(head) {
    if (!head) return head;

    function flattenDFS(node) {
        let current = node;
        let last = null;

        while (current) {
            if (current.child) {
                const next = current.next;
                const childLast = flattenDFS(current.child);

                // Link current node to child
                current.next = current.child;
                current.child.prev = current;

                // Link child's last node to next
                if (next) {
                    childLast.next = next;
                    next.prev = childLast;
                }

                // Nullify child pointer after flattening
                current.child = null;
                last = childLast;
            } else {
                last = current;
            }
            current = current.next;
        }
        return last;
    }

    flattenDFS(head);
    return head;
}
```

---

## Iterative Approach with Stack

### Intuition

In the iterative approach, a stack can be utilized to simulate the recursive stack we used in the recursive approach. The stack helps manage the process of flattening the list by holding nodes that we need to return to after diving into a child node's sublist.

### Steps

1. Initialize a stack and push the head node onto the stack.
2. Traverse through the stack:
   - Pop a node from the stack.
   - Append any existing children to the stack.
   - If there is a next node, append it to the stack as well.
   - Reconnect the `prev` and `next` pointers appropriately.
3. Continue this process until the stack is empty, ensuring that the multilevel list becomes fully flat.

### Time Complexity

- O(N) as we process every node once.

### Space Complexity

- O(N) to store nodes in the stack in the worst case, all nodes are on a single level with nested children.

### Code

```javascript
function flatten(head) {
    if (!head) return head;

    const stack = [];
    let current = head;

    while (current) {
        // If there's a child, we'll need to go deeper
        if (current.child) {
            // If there's a next, we'll come back to it later
            if (current.next) {
                stack.push(current.next);
            }

            // Connect current node to the child, flattening as we go
            current.next = current.child;
            current.next.prev = current;
            current.child = null;
        }

        // If this node has no next, and there are still nodes in the stack, pop from stack and continue flattening
        if (!current.next && stack.length > 0) {
            current.next = stack.pop();
            current.next.prev = current;
        }

        // Move to the next node
        current = current.next;
    }

    return head;
}
```


