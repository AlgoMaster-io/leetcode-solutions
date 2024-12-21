# [Leetcode 160: Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Hash Set](#approach-2-hash-set)
- [Approach 3: Two Pointers](#approach-3-two-pointers)

## Approach 1: Brute Force

### Intuition
The simplest way to determine if two linked lists intersect is to compare every node in the first linked list against every node in the second linked list. If a match is found, then that node is the intersection node.

### Steps
1. Start with the head of the first linked list.
2. For each node in the first linked list, iterate through all the nodes in the second linked list.
3. Check if any node in the second linked list matches the current node in the first linked list.
4. If a match is found, return that node.
5. If no match is found after all comparisons, return `null`.

### Code

```javascript
function getIntersectionNode(headA, headB) {
    let currentA = headA;

    // Iterate over each node in list A
    while (currentA !== null) {
        let currentB = headB;

        // Compare current node of A with each node in B
        while (currentB !== null) {
            if (currentA === currentB) {
                // Intersection found
                return currentA;
            }
            currentB = currentB.next;
        }
        currentA = currentA.next;
    }

    // No intersection found
    return null;
}
```

### Time Complexity
- **O(m * n)**, where `m` and `n` are the lengths of the two linked lists.
  
### Space Complexity
- **O(1)**, no additional space used.

## Approach 2: Hash Set

### Intuition
We can use a hash set to store all the nodes of the first linked list and then check if any node from the second linked list appears in this set. This efficiently checks for node intersection.

### Steps
1. Traverse the first linked list and store each node's reference in a hash set.
2. Traverse the second linked list and check if any node's reference is in the hash set.
3. The first node found in the hash set is the intersection.
4. If no node from the second linked list is found in the hash set, return `null`.

### Code

```javascript
function getIntersectionNode(headA, headB) {
    const nodesSet = new Set();
    let currentA = headA;

    // Store references of all nodes in list A in the set
    while (currentA !== null) {
        nodesSet.add(currentA);
        currentA = currentA.next;
    }

    let currentB = headB;

    // Check each node in list B against the set
    while (currentB !== null) {
        if (nodesSet.has(currentB)) {
            // Intersection found
            return currentB;
        }
        currentB = currentB.next;
    }

    // No intersection found
    return null;
}
```

### Time Complexity
- **O(m + n)**, where `m` and `n` are the lengths of the two linked lists.

### Space Complexity
- **O(m)**, space used by the hash set storing nodes of the first list.

## Approach 3: Two Pointers

### Intuition
A clever trick is to use two pointers to traverse the linked lists. The idea is to switch heads when the end of a list is reached. This means both pointers will traverse the same number of nodes when combined. If there is an intersection, they will meet at the intersection node after the same number of iterations.

### Steps
1. Initialize two pointers, `pointerA` and `pointerB`, at the heads of `headA` and `headB`.
2. Traverse both lists with each pointer moving to the next node.
3. When a pointer reaches the end of a list, redirect it to the head of the other list.
4. If the pointers are ever equal, that is the intersection node.
5. If both pointers reach `null`, there is no intersection.

### Code

```javascript
function getIntersectionNode(headA, headB) {
    if (headA === null || headB === null) return null;

    let pointerA = headA;
    let pointerB = headB;

    while (pointerA !== pointerB) {
        // Switch heads when end is reached
        pointerA = (pointerA === null) ? headB : pointerA.next;
        pointerB = (pointerB === null) ? headA : pointerB.next;
    }

    // Intersection node or null
    return pointerA;
}
```

### Time Complexity
- **O(m + n)**, where `m` and `n` are the lengths of the two linked lists. Each pointer traverses at most 2m or 2n nodes.

### Space Complexity
- **O(1)**, functional constant space use.

Each of these approaches offers a different trade-off between time complexity, space complexity, and code simplicity. The Two Pointers method is usually preferred for its optimal usage of both time and space.

