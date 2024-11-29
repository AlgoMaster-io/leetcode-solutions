# [148. Sort List](https://leetcode.com/problems/sort-list/)

## Approach 1: Merge Sort (Top-Down)

### Solution
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(log n) (due to recursion stack)

class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = val ?? 0;
        this.next = next ?? null;
    }
}

function sortList(head: ListNode | null): ListNode | null {
    if (!head || !head.next) {
        return head; // Base case: single node or empty list
    }

    // Split the list into two halves
    const mid = getMiddle(head);
    const rightHead = mid.next;
    mid.next = null;

    // Recursively sort each half
    const left = sortList(head);
    const right = sortList(rightHead);

    // Merge the sorted halves
    return merge(left, right);
}

function getMiddle(head: ListNode): ListNode {
    let slow = head, fast = head;
    while (fast.next && fast.next.next) {
        slow = slow.next!;
        fast = fast.next.next;
    }
    return slow;
}

function merge(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    const dummy = new ListNode(-1);
    let current = dummy;

    while (l1 && l2) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next!;
    }

    // Attach the remaining nodes
    current.next = l1 ? l1 : l2;

    return dummy.next;
}
```

## Approach 2: Merge Sort (Bottom-Up)

### Solution
```typescript
// Time Complexity: O(n log n)
// Space Complexity: O(1)

class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = val ?? 0;
        this.next = next ?? null;
    }
}

function sortList(head: ListNode | null): ListNode | null {
    if (!head || !head.next) {
        return head; // Base case: single node or empty list
    }

    // Get the length of the list
    let length = 0;
    let current = head;
    while (current) {
        length++;
        current = current.next;
    }

    const dummy = new ListNode(0);
    dummy.next = head;

    // Bottom-up merge sort
    for (let step = 1; step < length; step *= 2) {
        let prev = dummy;
        let curr = dummy.next;

        while (curr) {
            // Split the list into two halves of size `step`
            const left = curr;
            const right = split(left, step);
            curr = split(right, step);

            // Merge the two halves and attach to the sorted list
            prev.next = merge(left, right);

            // Move `prev` to the end of the merged segment
            while (prev.next) {
                prev = prev.next;
            }
        }
    }

    return dummy.next;
}

function split(head: ListNode | null, size: number): ListNode | null {
    for (let i = 1; head && i < size; i++) {
        head = head.next;
    }

    if (!head) {
        return null;
    }

    const next = head.next;
    head.next = null; // Disconnect the segment
    return next;
}

function merge(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    const dummy = new ListNode(-1);
    let current = dummy;

    while (l1 && l2) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next!;
    }

    current.next = l1 ? l1 : l2;

    return dummy.next;
}
```

