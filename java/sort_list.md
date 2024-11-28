# [148. Sort List](https://leetcode.com/problems/sort-list/)

## Approach 1: Merge Sort (Top-Down)

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(log n) (due to recursion stack)
public class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) {
            return head; // Base case: single node or empty list
        }

        // Split the list into two halves
        ListNode mid = getMiddle(head);
        ListNode rightHead = mid.next;
        mid.next = null;

        // Recursively sort each half
        ListNode left = sortList(head);
        ListNode right = sortList(rightHead);

        // Merge the sorted halves
        return merge(left, right);
    }

    private ListNode getMiddle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode current = dummy;

        while (l1 != null && l2 != null) {
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
        if (l1 != null) {
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
```java
// Time Complexity: O(n log n)
// Space Complexity: O(1)
public class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) {
            return head; // Base case: single node or empty list
        }

        // Get the length of the list
        int length = 0;
        ListNode current = head;
        while (current != null) {
            length++;
            current = current.next;
        }

        ListNode dummy = new ListNode(0);
        dummy.next = head;

        // Bottom-up merge sort
        for (int step = 1; step < length; step *= 2) {
            ListNode prev = dummy;
            ListNode curr = dummy.next;

            while (curr != null) {
                // Split the list into two halves of size `step`
                ListNode left = curr;
                ListNode right = split(left, step);
                curr = split(right, step);

                // Merge the two halves and attach to the sorted list
                prev.next = merge(left, right);

                // Move `prev` to the end of the merged segment
                while (prev.next != null) {
                    prev = prev.next;
                }
            }
        }

        return dummy.next;
    }

    private ListNode split(ListNode head, int size) {
        for (int i = 1; head != null && i < size; i++) {
            head = head.next;
        }

        if (head == null) {
            return null;
        }

        ListNode next = head.next;
        head.next = null; // Disconnect the segment
        return next;
    }

    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode current = dummy;

        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }

        if (l1 != null) {
            current.next = l1;
        } else {
            current.next = l2;
        }

        return dummy.next;
    }
}
```