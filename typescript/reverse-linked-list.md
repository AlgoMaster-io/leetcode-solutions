[Leetcode 206: Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

### Navigation
- [Approach 1: Iterative Approach](#approach-1-iterative-approach)
- [Approach 2: Recursive Approach](#approach-2-recursive-approach)

## Approach 1: Iterative Approach

### Intuition:
The iterative approach uses pointers to reverse the linked list. Starting with the head, for each node, you need to reverse the link to point to the previous node instead of the next one. You will keep track of the previous node, current node, and the next node temporarily as you iterate through the list.

```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = val === undefined ? 0 : val;
        this.next = next === undefined ? null : next;
    }
}

function reverseList(head: ListNode | null): ListNode | null {
    let prev: ListNode | null = null;
    let curr: ListNode | null = head;
    
    while (curr !== null) {
        // Store the next node temporarily
        let nextTemp: ListNode | null = curr.next;

        // Reverse the link
        curr.next = prev;

        // Move previous and current pointers one step forward
        prev = curr;
        curr = nextTemp;
    }
    // At the end, prev will be the new head of the reversed linked list
    return prev;
}
```

**Time Complexity:** O(n), where n is the number of nodes in the linked list. We process each node exactly once.

**Space Complexity:** O(1), no additional space is used.

---

## Approach 2: Recursive Approach

### Intuition:
The recursive approach is more elegant and involves reversing the rest of the list recursively and then fixing the head pointer. For each call, after reversing the smaller list, the head of the list needs to point its next node to itself in reverse order.

```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = val === undefined ? 0 : val;
        this.next = next === undefined ? null : next;
    }
}

function reverseList(head: ListNode | null): ListNode | null {
    // Base case: if head is null or there's only one node
    if (head === null || head.next === null) {
        return head;
    }

    // Recursively reverse the smaller list
    const reversedListHead = reverseList(head.next);

    // Reverse the current node and the next node
    head.next.next = head; // Next node points back to the current node
    head.next = null; // Set the current node's next to null

    // Return the head of the reversed list
    return reversedListHead;
}
```

**Time Complexity:** O(n), where n is the number of nodes in the linked list. Each node is visited exactly once.

**Space Complexity:** O(n), due to the recursion stack, which can go as deep as the number of nodes in the list.

