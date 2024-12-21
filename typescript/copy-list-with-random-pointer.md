# [Leetcode 138: Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

## Approaches:
1. [Iterative with Hash Map](#iterative-with-hash-map)
2. [Optimized with Interweaving Nodes](#optimized-with-interweaving-nodes)

## Iterative with Hash Map

### Intuition:
The challenge with this problem lies in the presence of random pointers, which are independent of the list's standard next pointers. A naive solution would ignore the random pointers, but a correct approach must keep track of them.

The idea here is to make use of a hash map to map each original node to its clone. This makes it easy to handle the random pointers, since we can just look up the original node's clone using the map.

### Algorithm:
1. Traverse the original list and create a copy of each node (only values initially).
2. Store the mapping from each node to its copy in a hash map.
3. Traverse the list again to adjust the `next` and `random` pointers for each copied node using the hash map.

### Code:
```typescript
class Node {
    val: number;
    next: Node | null;
    random: Node | null;
    constructor(val?: number, next?: Node, random?: Node) {
        this.val = (val === undefined ? 0 : val);
        this.next = (next === undefined ? null : next);
        this.random = (random === undefined ? null : random);
    }
}

function copyRandomList(head: Node | null): Node | null {
    if (!head) return null;

    const nodeMap = new Map<Node, Node>();

    let current = head;
    // First pass: copy all nodes and store in map
    while (current) {
        nodeMap.set(current, new Node(current.val));
        current = current.next;
    }

    current = head;
    // Second pass: set the copied node pointers
    while (current) {
        const copiedNode = nodeMap.get(current);
        copiedNode.next = current.next ? nodeMap.get(current.next) : null;
        copiedNode.random = current.random ? nodeMap.get(current.random) : null;
        current = current.next;
    }

    return nodeMap.get(head);
}
```

### Complexity:
- **Time Complexity**: O(N), where N is the number of nodes in the linked list. We iterate through the list twice.
- **Space Complexity**: O(N), due to the space used by the hash map to store the node mappings.

## Optimized with Interweaving Nodes

### Intuition:
Instead of using a separate hash map, we can cleverly interweave the copied nodes with the original nodes in the list. This will allow us to set the `random` pointers without the need for extra space for the map.

### Algorithm:
1. First, iterate through the list and create new nodes interweaved with original nodes, i.e., `original.next = copy`.
2. For each node in the list, set the `random` of the copied node.
3. Finally, separate the original and copied lists.

### Code:
```typescript
function copyRandomListOptimized(head: Node | null): Node | null {
    if (!head) return null;

    let current = head;
    // First pass: interleave copied nodes between original nodes
    while (current) {
        const copy = new Node(current.val);
        copy.next = current.next;
        current.next = copy;
        current = copy.next;
    }

    current = head;
    // Second pass: set the random pointers for the copied nodes
    while (current) {
        if (current.random) {
            current.next.random = current.random.next;
        }
        current = current.next ? current.next.next : null;
    }

    current = head;
    const pseudoHead = new Node(0);
    let copyCurrent = pseudoHead;

    // Third pass: separate original and copied nodes
    while (current) {
        const copiedNode = current.next;
        current.next = copiedNode.next;
        copyCurrent.next = copiedNode;
        copyCurrent = copiedNode;
        current = current.next;
    }

    return pseudoHead.next;
}
```

### Complexity:
- **Time Complexity**: O(N), we traverse the list a constant number of times.
- **Space Complexity**: O(1), since we are not using additional space proportional to the input size, and the interweaving leaves the original list intact.

