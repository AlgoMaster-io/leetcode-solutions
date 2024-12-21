# [Leetcode 61: Rotate List](https://leetcode.com/problems/rotate-list/)

## Approaches

1. [Brute Force Rotate](#brute-force-rotate)
2. [Using Array Rearrangement](#using-array-rearrangement)
3. [Optimized Using Linked List Manipulation](#optimized-using-linked-list-manipulation)

---

## Brute Force Rotate

### Intuition

A simple approach to rotate a list is to remove the last element `k` times and insert it at the beginning of the list. This method directly translates to iterating through the linked list and making adjustments step by step.

### Steps

1. Traverse the list to find the length.
2. For each rotation, traverse to the last node.
3. Disconnect the last node and move it to the front.
4. Repeat the above steps for `k` times.

### Code

```javascript
function rotateRight(head, k) {
    if (!head || !head.next || k === 0) return head;

    // Calculate the length
    let length = 1;
    let tail = head;
    while (tail.next) {
        tail = tail.next;
        length++;
    }

    // Normalize k
    k = k % length;
    if (k === 0) return head;

    for (let i = 0; i < k; i++) {
        let prev = null;
        let current = head;
        // Find the second last node
        while (current.next) {
            prev = current;
            current = current.next;
        }

        // Insert last node at the front
        prev.next = null;
        current.next = head;
        head = current;
    }

    return head;
}
```

### Time and Space Complexity

- **Time Complexity**: O(n * k), where n is the number of nodes in the list. The for loop explicitly runs `k` times, each time needing to go through the list.
- **Space Complexity**: O(1), as no extra space is used except pointers.

---

## Using Array Rearrangement

### Intuition

Another approach is to first convert the linked list into an array to easily manipulate the order and then reconstruct the linked list from the array.

### Steps

1. Traverse the linked list and store the nodes in an array.
2. Rotate the array based on the modulo of `k` and the length.
3. Build the linked list back from the array.

### Code

```javascript
function rotateRight(head, k) {
    if (!head || k === 0) return head;

    const nodes = [];
    let current = head;

    // Store nodes in an array
    while (current) {
        nodes.push(current);
        current = current.next;
    }

    const n = nodes.length;
    k = k % n;
    if (k === 0) return head;

    // Rearrange the array
    const rotatedPart = nodes.splice(n - k, k);
    nodes.unshift(...rotatedPart);

    // Reconstruct the linked list
    head = nodes[0];
    current = head;
    for (let i = 1; i < nodes.length; i++) {
        current.next = nodes[i];
        current = current.next;
    }
    current.next = null;

    return head;
}
```

### Time and Space Complexity

- **Time Complexity**: O(n), where n is the number of nodes in the list.
- **Space Complexity**: O(n), because we're using an extra array to store the nodes.

---

## Optimized Using Linked List Manipulation

### Intuition

The optimal solution involves understanding that a rotation is about moving the end part of the list to the front. The trick is to efficiently find the new tail and new head without repeatedly traversing the list.

### Steps

1. Find the length and simultaneously link the last node to the head, forming a circular list.
2. Normalize `k` using modulo. Find the position to break the circular list to form the new list.
3. Traverse to the node just before the new head, update pointers to form the list.

### Code

```javascript
function rotateRight(head, k) {
    if (!head || !head.next || k === 0) return head;

    // First calculate the length
    let length = 1;
    let oldTail = head;
    while (oldTail.next) {
        oldTail = oldTail.next;
        length++;
    }

    // Form a circular list
    oldTail.next = head;

    // Find new tail and new head
    k = k % length;
    let newTail = head;
    for (let i = 0; i < length - k - 1; i++) {
        newTail = newTail.next;
    }
    let newHead = newTail.next;

    // Break the circle
    newTail.next = null;

    return newHead;
}
```

### Time and Space Complexity

- **Time Complexity**: O(n), a full traversal is needed to compute the length and rearrange the pointers.
- **Space Complexity**: O(1), as it only uses constant extra space.

This solution minimizes the need to repeatedly traverse the list, resulting in a more optimal approach to the problem.

