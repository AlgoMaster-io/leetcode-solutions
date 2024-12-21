# [Leetcode 876: Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approaches

1. [Two-pass Count and Retrieve](#two-pass-count-and-retrieve)
2. [Two-pointer (Tortoise and Hare)](#two-pointer-tortoise-and-hare)

## Two-pass Count and Retrieve

### Intuition
- The first straightforward solution is to traverse the linked list once to count the number of nodes. 
- In the second pass, traverse the list again until you reach the node that is at the middle. This is calculated as floor of count/2.

### Code

```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.next = (next===undefined ? null : next);
    }
}

function middleNode(head: ListNode | null): ListNode | null {
    let count = 0;
    let current = head;
    
    // Count the total number of nodes
    while (current !== null) {
        count++;
        current = current.next;
    }
    
    // Calculate the middle index
    const mid = Math.floor(count / 2);
    current = head;
    
    // Move to the middle node
    for (let i = 0; i < mid; i++) {
        current = current!.next;
    }
    
    return current;
}
```

### Complexity
- **Time complexity**: O(n), where n is the number of nodes in the linked list. This is because we traverse the list twice.
- **Space complexity**: O(1), as we are using only a constant amount of space for variables.

## Two-pointer (Tortoise and Hare)

### Intuition
- This approach utilizes two pointers moving at different speeds (known as the Tortoise and Hare method).
- The slow pointer moves one step at a time, while the fast pointer moves two steps at a time.
- When the fast pointer reaches the end of the list, the slow pointer will be at the middle.

### Code

```typescript
function middleNode(head: ListNode | null): ListNode | null {
    let slow = head;
    let fast = head;
    
    // Move fast pointer by 2 steps and slow pointer by 1 step
    while (fast !== null && fast.next !== null) {
        slow = slow!.next;
        fast = fast.next.next;
    }
    
    // When fast reaches the end, slow will be at the middle
    return slow;
}
```

### Complexity
- **Time complexity**: O(n), where n is the number of nodes in the linked list. We traverse each node only once.
- **Space complexity**: O(1), as the space is used only for the two pointers.

This solution is optimal in terms of both time and space complexities compared to the first approach, as it requires only a single pass through the linked list and uses constant space.

