# [148. Sort List](https://leetcode.com/problems/sort-list/)

## Approach 1: Merge Sort (Top-Down)

### Solution
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(log n) (due to recursion stack)
class Solution {
    sortList(head) {
        if (head === null || head.next === null) {
            return head; // Base case: single node or empty list
        }

        // Split the list into two halves
        const mid = this.getMiddle(head);
        const rightHead = mid.next;
        mid.next = null;

        // Recursively sort each half
        const left = this.sortList(head);
        const right = this.sortList(rightHead);

        // Merge the sorted halves
        return this.merge(left, right);
    }

    getMiddle(head) {
        let slow = head, fast = head;
        while (fast.next !== null && fast.next.next !== null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    merge(l1, l2) {
        const dummy = new ListNode(-1);
        let current = dummy;

        while (l1 !== null && l2 !== null) {
            if (l1.val < l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }

        // Attach the remaining nodes
        if (l1 !== null) {
            current.next = l1;
        } else {
            current.next = l2;
        }

        return dummy.next;
    }
}
```

## Approach 2: Merge Sort (Bottom-Up)

### Solution
```javascript
// Time Complexity: O(n log n)
// Space Complexity: O(1)
class Solution {
    sortList(head) {
        if (head === null || head.next === null) {
            return head; // Base case: single node or empty list
        }

        // Get the length of the list
        let length = 0;
        let current = head;
        while (current !== null) {
            length++;
            current = current.next;
        }

        const dummy = new ListNode(0);
        dummy.next = head;

        // Bottom-up merge sort
        for (let step = 1; step < length; step *= 2) {
            let prev = dummy;
            let curr = dummy.next;

            while (curr !== null) {
                // Split the list into two halves of size `step`
                const left = curr;
                const right = this.split(left, step);
                curr = this.split(right, step);

                // Merge the two halves and attach to the sorted list
                prev.next = this.merge(left, right);

                // Move `prev` to the end of the merged segment
                while (prev.next !== null) {
                    prev = prev.next;
                }
            }
        }

        return dummy.next;
    }

    split(head, size) {
        for (let i = 1; head !== null && i < size; i++) {
            head = head.next;
        }

        if (head === null) {
            return null;
        }

        const next = head.next;
        head.next = null; // Disconnect the segment
        return next;
    }

    merge(l1, l2) {
        const dummy = new ListNode(-1);
        let current = dummy;

        while (l1 !== null && l2 !== null) {
            if (l1.val < l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }

        if (l1 !== null) {
            current.next = l1;
        } else {
            current.next = l2;
        }

        return dummy.next;
    }
}
```

