# [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approach 1: Two Passes

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function removeNthFromEnd(head, n) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let length = 0;

    // Calculate the total length of the list
    let current = head;
    while (current !== null) {
        length++;
        current = current.next;
    }

    // Find the node before the one to remove
    let target = length - n;
    current = dummy;
    for (let i = 0; i < target; i++) {
        current = current.next;
    }

    // Remove the nth node
    current.next = current.next.next;

    return dummy.next;
}
```

## Approach 2: One Pass with Two Pointers

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function removeNthFromEnd(head, n) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let first = dummy;
    let second = dummy;

    // Move the first pointer n + 1 steps ahead
    for (let i = 0; i <= n; i++) {
        first = first.next;
    }

    // Move both pointers until the first reaches the end
    while (first !== null) {
        first = first.next;
        second = second.next;
    }

    // Remove the nth node
    second.next = second.next.next;

    return dummy.next;
}
```

