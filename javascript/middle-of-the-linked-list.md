# [Leetcode 876: Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approaches
1. [Two Pass Approach](#two-pass-approach)
2. [Single Pass with Fast and Slow Pointer](#single-pass-with-fast-and-slow-pointer)

---

## Two Pass Approach

### Intuition
The idea is to find the length of the linked list in the first pass, and then in the second pass, move to the middle of the list using the length we calculated. 

### Approach
1. Traverse the linked list to calculate the total number of nodes (`length`).
2. Compute the middle index, which is `Math.floor(length / 2)`.
3. Traverse the linked list a second time, stopping at the middle index to return the middle node.

### Code
```javascript
function middleNode(head) {
    let currentNode = head;
    let length = 0;

    // First pass: Calculate total length of the linked list
    while (currentNode !== null) {
        length++;
        currentNode = currentNode.next;
    }

    // Calculate middle index
    const middleIndex = Math.floor(length / 2);

    // Second pass: Traverse to the middle node
    currentNode = head;
    for (let i = 0; i < middleIndex; i++) {
        currentNode = currentNode.next;
    }

    return currentNode;
}
```

### Time Complexity
- **O(n)**: We traverse the linked list twice, each taking `O(n)` time.
  
### Space Complexity
- **O(1)**: We use a constant amount of extra space.


---

## Single Pass with Fast and Slow Pointer

### Intuition
Using two pointers, `fast` and `slow`, significantly optimizes the solution. The `fast` pointer moves twice as fast as the `slow` pointer. By the time the `fast` pointer reaches the end of the list, the `slow` pointer will be at the middle.

### Approach
1. Initialize two pointers, `slow` and `fast`, at the head.
2. Move `slow` one step and `fast` two steps in each iteration.
3. When `fast` reaches the end, `slow` will be at the middle of the linked list.

### Code
```javascript
function middleNode(head) {
    let slow = head;
    let fast = head;

    // Move fast 2 steps and slow 1 step forward
    while (fast !== null && fast.next !== null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;
}
```

### Time Complexity
- **O(n)**: We traverse the linked list in one pass up to the middle.
  
### Space Complexity
- **O(1)**: No additional data structures are used, only pointers.

---

