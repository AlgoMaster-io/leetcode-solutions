# [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

## Approach 1: Iterative Solution

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

function reverseList(head: ListNode | null): ListNode | null {
    let prev: ListNode | null = null; // Previous node, initially null
    let current: ListNode | null = head; // Current node to process

    while (current !== null) {
        const nextTemp: ListNode | null = current.next; // Store the next node
        current.next = prev; // Reverse the link
        prev = current; // Move prev to the current node
        current = nextTemp; // Move to the next node
    }

    return prev; // New head of the reversed list
}
```

## Approach 2: Recursive Solution

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n) (due to recursion stack)

class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.next = (next === undefined ? null : next);
    }
}

function reverseList(head: ListNode | null): ListNode | null {
    // Base case: if the list is empty or has only one node
    if (head === null || head.next === null) {
        return head;
    }

    // Reverse the rest of the list
    const reversedList: ListNode | null = reverseList(head.next);

    // Reverse the current node
    head.next.next = head;
    head.next = null;

    return reversedList; // Return the new head
}
```

