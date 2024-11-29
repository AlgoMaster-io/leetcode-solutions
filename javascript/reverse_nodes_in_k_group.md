# [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approach: Iterative Solution with Dummy Node

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

function reverseKGroup(head, k) {
    if (head === null || k === 1) {
        return head;
    }

    const dummy = new ListNode(0);
    dummy.next = head;
    let prevGroup = dummy;

    while (true) {
        // Find the k-th node from prevGroup
        const kthNode = getKthNode(prevGroup, k);
        if (kthNode === null) {
            break; // Not enough nodes to reverse
        }

        const nextGroup = kthNode.next; // Node after the k-th node
        let prev = nextGroup; // Start reversal with nextGroup as tail
        let current = prevGroup.next;

        // Reverse k nodes
        while (current !== nextGroup) {
            const temp = current.next;
            current.next = prev;
            prev = current;
            current = temp;
        }

        // Update the connections
        const temp = prevGroup.next;
        prevGroup.next = kthNode;
        prevGroup = temp;
    }

    return dummy.next;
}

function getKthNode(start, k) {
    while (start !== null && k > 0) {
        start = start.next;
        k--;
    }
    return start;
}
```

