# [148. Sort List](https://leetcode.com/problems/sort-list/)

## Approach 1: Merge Sort (Top-Down)

### Solution
python
```python
# Time Complexity: O(n log n)
# Space Complexity: O(log n) (due to recursion stack)

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head  # Base case: single node or empty list

        # Split the list into two halves
        mid = self.getMiddle(head)
        rightHead = mid.next
        mid.next = None

        # Recursively sort each half
        left = self.sortList(head)
        right = self.sortList(rightHead)

        # Merge the sorted halves
        return self.merge(left, right)

    def getMiddle(self, head: ListNode) -> ListNode:
        slow = head
        fast = head
        while fast.next and fast.next.next:
            slow = slow.next
            fast = fast.next.next
        return slow

    def merge(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode(-1)
        current = dummy

        while l1 and l2:
            if l1.val < l2.val:
                current.next = l1
                l1 = l1.next
            else:
                current.next = l2
                l2 = l2.next
            current = current.next

        # Attach the remaining nodes
        current.next = l1 if l1 else l2

        return dummy.next
```

## Approach 2: Merge Sort (Bottom-Up)

### Solution
python
```python
# Time Complexity: O(n log n)
# Space Complexity: O(1)

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head  # Base case: single node or empty list

        # Get the length of the list
        def get_length(node):
            count = 0
            while node:
                count += 1
                node = node.next
            return count

        length = get_length(head)
        dummy = ListNode(0)
        dummy.next = head

        # Bottom-up merge sort
        step = 1
        while step < length:
            prev = dummy
            curr = dummy.next
            while curr:
                # Split the list into two halves of size `step`
                left = curr
                right = self.split(left, step)
                curr = self.split(right, step)

                # Merge the two halves and attach to the sorted list
                prev.next = self.merge(left, right)

                # Move `prev` to the end of the merged segment
                while prev.next:
                    prev = prev.next

            step *= 2

        return dummy.next

    def split(self, head: ListNode, size: int) -> ListNode:
        for i in range(1, size):
            if not head:
                break
            head = head.next

        if not head:
            return None

        next_part = head.next
        head.next = None  # Disconnect the segment
        return next_part

    def merge(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode(-1)
        current = dummy

        while l1 and l2:
            if l1.val < l2.val:
                current.next = l1
                l1 = l1.next
            else:
                current.next = l2
                l2 = l2.next
            current = current.next

        current.next = l1 if l1 else l2

        return dummy.next
```

