# [Leetcode 430: Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)

## Approaches
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Iterative Approach using Stack](#approach-2-iterative-approach-using-stack)

---

## Approach 1: Recursive Approach

### Intuition
The problem requires flattening a multilevel doubly linked list. The list has `child` pointers which can point to another list. Our aim is to eliminate these `child` pointers by integrating the child lists into the main list. A recursive strategy is natural here: if we visit a node with a `child`, we recursively flatten the child list and attach it between the current node and its next node.

### Steps
1. Base case: If the node is `null`, return immediately as there is nothing to flatten.
2. For each node, check if there's a `child` node.
3. If there is a `child`, recursively flatten this list.
4. Attach the flattened `child` list between the current node and its `next` node.
5. Update the pointers accordingly to ensure the list remains doubly linked.
6. Continue the process till the end of the list.

### Code
```typescript
class Node {
    val: number;
    prev: Node | null;
    next: Node | null;
    child: Node | null;
    constructor(val?: number, prev?: Node, next?: Node, child?: Node) {
        this.val = (val===undefined ? 0 : val);
        this.prev = (prev===undefined ? null : prev);
        this.next = (next===undefined ? null : next);
        this.child = (child===undefined ? null : child);
    }
}

function flatten(head: Node | null): Node | null {
    if (!head) return null;

    const flattenList = (node: Node | null): Node => {
        let current = node;
        let tail = node;

        while (current) {
            const next = current.next;
            if (current.child) {
                const childTail = flattenList(current.child);
                
                current.next = current.child;  // Link child to current node
                current.child.prev = current;
                current.child = null;
                
                if (next) {
                    childTail.next = next;  // Link the end of child to the next node
                    next.prev = childTail;
                }

                tail = childTail;  // Update the tail to childTail after flattening child
            } else {
                tail = current;  // Update tail in case no child to flatten
            }
            
            current = next;
        }
        
        return tail;
    };

    flattenList(head);
    return head;
}
```

### Complexity
- **Time complexity**: \(O(n)\), where \(n\) is the total number of nodes in the list. Each node in the list is visited exactly once.  
- **Space complexity**: \(O(d)\), where \(d\) is the depth of the multilevel list, due to the recursion stack.

---

## Approach 2: Iterative Approach using Stack

### Intuition
Another effective method for flattening the list is to use a stack. This helps simulate the recursive process iteratively. We'll push nodes onto the stack whenever we need to explore a `child` or `next` node, retracing our steps and integrating nodes when necessary.

### Steps
1. Traverse the list using a stack to store nodes.
2. Add `next` nodes onto the stack if a `child` node is encountered.
3. Connect `child` nodes directly after the current node and before any `next` nodes, modifying pointers appropriately.
4. Continue popping nodes off the stack until it's empty, rebuilding the main list.

### Code
```typescript
function flatten(head: Node | null): Node | null {
    if (!head) return null;

    let current: Node | null = head;
    const stack: Node[] = [];
    
    while (current) {
        if (current.child) {
            if (current.next) {
                stack.push(current.next);  // Save the next node to revisit later
            }
            current.next = current.child;
            current.child.prev = current;
            current.child = null;
        }
        
        if (!current.next && stack.length > 0) {
            current.next = stack.pop()!;
            current.next.prev = current;
        }
        
        current = current.next;
    }
    
    return head;
}
```

### Complexity
- **Time complexity**: \(O(n)\), where \(n\) is the total number of nodes in the list, as we visit each node once.
- **Space complexity**: \(O(d)\), where \(d\) is the depth of the multilevel list, due to the stack storing nodes at each level.

