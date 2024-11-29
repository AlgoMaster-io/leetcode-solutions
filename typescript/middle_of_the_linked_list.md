# [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approach 1: Two Passes (Count Nodes)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)

class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.next = (next === undefined ? null : next);
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

    // Find the middle index
    const middleIndex = Math.floor(count / 2);
    current = head;

    // Move to the middle node
    for (let i = 0; i < middleIndex; i++) {
        current = (current as ListNode).next;
    }

    return current;
}
```

## Approach 2: Fast and Slow Pointers (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)

function middleNode(head: ListNode | null): ListNode | null {
    let slow = head;
    let fast = head;

    // Move fast pointer twice as fast as the slow pointer
    while (fast !== null && fast.next !== null) {
        slow = (slow as ListNode).next; // Move slow pointer one step
        fast = (fast.next as ListNode).next; // Move fast pointer two steps
    }

    return slow; // Slow pointer will be at the middle
}
```

