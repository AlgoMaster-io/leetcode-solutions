# [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approach 1: Two Passes

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        dummy = ListNode(0)
        dummy.next = head
        length = 0

        # Calculate the total length of the list
        current = head
        while current is not None:
            length += 1
            current = current.next

        # Find the node before the one to remove
        target = length - n
        current = dummy
        for i in range(target):
            current = current.next

        # Remove the nth node
        current.next = current.next.next

        return dummy.next
```

## Approach 2: One Pass with Two Pointers

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        dummy = ListNode(0)
        dummy.next = head
        first = dummy
        second = dummy

        # Move the first pointer n + 1 steps ahead
        for i in range(n + 1):
            first = first.next

        # Move both pointers until the first reaches the end
        while first is not None:
            first = first.next
            second = second.next

        # Remove the nth node
        second.next = second.next.next

        return dummy.next
```

