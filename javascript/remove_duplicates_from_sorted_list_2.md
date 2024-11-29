# [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## Approach: Dummy Node and Two Pointers

### Solution

```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)

class ListNode {
    constructor(val, next = null) {
        this.val = val;
        this.next = next;
    }
}

function deleteDuplicates(head) {
    const dummy = new ListNode(0); // Dummy node to handle edge cases
    dummy.next = head;
    let prev = dummy; // Pointer to track the node before duplicates

    while (head !== null) {
        // Check for duplicates
        if (head.next !== null && head.val === head.next.val) {
            // Skip all nodes with the same value
            while (head.next !== null && head.val === head.next.val) {
                head = head.next;
            }
            // Remove duplicates by linking prev to head's next
            prev.next = head.next;
        } else {
            // No duplicates, move prev forward
            prev = prev.next;
        }
        // Move head forward
        head = head.next;
    }

    return dummy.next;
}
```

