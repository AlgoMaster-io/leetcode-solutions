# [Leetcode 19: Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approaches:

1. [Two Pass Solution](#solution-1-two-pass-solution)
2. [One Pass Solution](#solution-2-one-pass-solution)

### Solution 1: Two Pass Solution

**Intuition:**

The basic idea of the two-pass solution is to first determine the length of the linked list. Once we have the length, we can easily identify the node that is the nth from the end by calculating its position from the start. Then, we remove that node.

**Steps:**

1. Traverse the list once to calculate its total length.
2. Calculate the position of the node to be removed from the start.
3. Traverse the list again to find that node.
4. Remove the node by changing the pointers.

**Code:**

```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = val === undefined ? 0 : val;
        this.next = next === undefined ? null : next;
    }
}

function removeNthFromEnd(head: ListNode | null, n: number): ListNode | null {
    let length = 0;
    let current = head;
    
    // First pass to find the length of the linked list
    while (current !== null) {
        length++;
        current = current.next;
    }

    // Determine the position to remove, from the start
    const positionToRemove = length - n;
    
    // Create a new dummy node to simplify edge cases
    const dummy = new ListNode(0, head);
    current = dummy;
    
    // Second pass to reach the node before the one we need to remove
    for (let i = 0; i < positionToRemove; i++) {
        current = current.next;
    }

    // Remove the nth node from the end
    current.next = current.next?.next || null;
    
    return dummy.next;
}
```

**Time Complexity:** O(L), where L is the number of nodes in the linked list (two complete traversals).

**Space Complexity:** O(1), constant space used.

---

### Solution 2: One Pass Solution

**Intuition:**

To optimize the solution into a single pass, use two pointers with a fixed gap between them. The key is to maintain the gap of 'n' nodes between two pointers. As soon as the first pointer reaches the end, the second pointer will be at the node just before the nth node from the end.

**Steps:**

1. Initialize two pointers, `first` and `second`, at the dummy node.
2. Move `first` forward by `n + 1` steps.
3. Move both pointers forward until `first` reaches the end.
4. The `second` pointer will be just before the target node.
5. Remove the target node by adjusting the pointers.

**Code:**

```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = val === undefined ? 0 : val;
        this.next = next === undefined ? null : next;
    }
}

function removeNthFromEnd(head: ListNode | null, n: number): ListNode | null {
    const dummy = new ListNode(0, head);
    let first: ListNode | null = dummy;
    let second: ListNode | null = dummy;

    // Move the first pointer so that the gap is n nodes apart from the second
    for (let i = 0; i <= n; i++) {
        if (first) first = first.next;
    }

    // Move first to the end, maintaining the gap
    while (first !== null) {
        first = first.next;
        second = second.next!;
    }

    // Skip the node that is the 'nth' from the end
    second.next = second.next?.next || null;

    return dummy.next;
}
```

**Time Complexity:** O(L), where L is the number of nodes in the list (one complete traversal).

**Space Complexity:** O(1), constant space used.

