# [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

## Approach: Iterative Solution with Dummy Node

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

const reverseBetween = function(head, left, right) {
    if (head === null || left === right) {
        return head;
    }

    const dummy = new ListNode(0); // Dummy node to simplify edge cases
    dummy.next = head;
    let prev = dummy;

    // Step 1: Move prev to the node before the "left" position
    for (let i = 1; i < left; i++) {
        prev = prev.next;
    }

    // Step 2: Reverse the sublist from "left" to "right"
    let current = prev.next; // The first node to be reversed
    let next = null; // Temporary node to store the next node
    let prevSublist = null; // Tracks the reversed sublist

    for (let i = 0; i <= right - left; i++) {
        next = current.next; // Store the next node
        current.next = prevSublist; // Reverse the current node
        prevSublist = current; // Move prevSublist to the current node
        current = next; // Move to the next node
    }

    // Step 3: Reconnect the reversed sublist with the rest of the list
    prev.next.next = current; // Connect the tail of the reversed sublist
    prev.next = prevSublist; // Connect the head of the reversed sublist

    return dummy.next; // Return the updated list
};
```

