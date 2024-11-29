# [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approach: Iterative Solution with Dummy Node

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)

class ListNode {
    val: number;
    next: ListNode | null;

    constructor(val?: number, next?: ListNode | null) {
        this.val = val === undefined ? 0 : val;
        this.next = next === undefined ? null : next;
    }
}

function reverseKGroup(head: ListNode | null, k: number): ListNode | null {
    if (head === null || k === 1) {
        return head;
    }

    const dummy: ListNode = new ListNode(0);
    dummy.next = head;
    let prevGroup: ListNode = dummy;

    while (true) {
        const kthNode: ListNode | null = getKthNode(prevGroup, k);
        if (kthNode === null) {
            break; // Not enough nodes to reverse
        }

        const nextGroup: ListNode | null = kthNode.next; // Node after the k-th node
        let prev: ListNode | null = nextGroup; // Start reversal with nextGroup as tail
        let current: ListNode | null = prevGroup.next;

        // Reverse k nodes
        while (current !== nextGroup) {
            const temp: ListNode | null = current!.next;
            current!.next = prev;
            prev = current;
            current = temp;
        }

        // Update the connections
        const temp: ListNode | null = prevGroup.next;
        prevGroup.next = kthNode;
        prevGroup = temp!;
    }

    return dummy.next;
}

function getKthNode(start: ListNode, k: number): ListNode | null {
    while (start !== null && k > 0) {
        start = start.next!;
        k--;
    }
    return start;
}
```

