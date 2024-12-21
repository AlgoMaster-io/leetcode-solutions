# [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

## Approaches
1. [Naive Approach: Using a HashMap](#naive-approach)
2. [Optimized Approach: Interweaving Nodes](#optimized-approach)

### Naive Approach

#### Intuition:
The problem involves a linked list where each node has a `next` pointer as in a typical singly linked list, and an additional `random` pointer which points to any node in the list or `null`. The goal is to create a deep copy of this list.

A straightforward approach to tackle this problem is to use a HashMap (or an object in JavaScript) to store the mapping between original nodes and their copies. This allows us to easily recreate both the `next` and `random` pointers.

#### Solution:
1. First, create a mapping for all nodes from the original list to their new copies.
2. Traverse the list a second time to update `next` and `random` pointers for each copied node using the map.

```javascript
function copyRandomList(head) {
    if (!head) return null;
    
    const map = new Map();
    
    // First pass: copy nodes and store them in the map
    let current = head;
    while (current) {
        map.set(current, new Node(current.val));
        current = current.next;
    }
    
    // Second pass: assign next and random pointers
    current = head;
    while (current) {
        const newNode = map.get(current);
        newNode.next = map.get(current.next) || null;
        newNode.random = map.get(current.random) || null;
        current = current.next;
    }
    
    // Return the head of the new linked list
    return map.get(head);
}
```

**Time Complexity:** O(N), where N is the number of nodes in the linked list, as we are iterating over the list twice.

**Space Complexity:** O(N), due to the additional space required to store the map.

### Optimized Approach: Interweaving Nodes

#### Intuition:
To optimize the space complexity such that no additional data structures are needed, we can temporarily interweave the original nodes with their copies in the same linked list.

#### Solution:
1. Interweave the copied nodes with the original nodes. For each node, create a copy and insert it just after the original node.
2. Assign correct `random` pointers for all new nodes.
3. Separate the interweaved list to return only the copied linked list.

```javascript
function copyRandomList(head) {
    if (!head) return null;

    // Step 1: Creating new nodes interleaved with original nodes
    let current = head;
    while (current) {
        let newNode = new Node(current.val, current.next, current.random);
        current.next = newNode;
        current = newNode.next;
    }

    // Step 2: Updating random pointers for the new nodes
    current = head;
    while (current) {
        if (current.random) {
            current.next.random = current.random.next;
        }
        current = current.next.next;
    }

    // Step 3: Restoring the original list and extracting the copied list
    current = head;
    let pseudoHead = new Node(0);
    let copyCurrent = pseudoHead;
    while (current) {
        let clone = current.next;
        current.next = clone.next;
        current = current.next;
        copyCurrent.next = clone;
        copyCurrent = clone;
    }

    return pseudoHead.next;
}
```

**Time Complexity:** O(N), as we are iterating over the list only three times.

**Space Complexity:** O(1), because we do not use any additional space except a few pointers.

