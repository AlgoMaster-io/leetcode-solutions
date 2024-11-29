# [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## Approach: Dummy Node and Two Pointers

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        dummy = ListNode(0)  # Dummy node to handle edge cases
        dummy.next = head
        prev = dummy  # Pointer to track the node before duplicates

        while head:
            # Check for duplicates
            if head.next and head.val == head.next.val:
                # Skip all nodes with the same value
                while head.next and head.val == head.next.val:
                    head = head.next
                # Remove duplicates by linking prev to head's next
                prev.next = head.next
            else:
                # No duplicates, move prev forward
                prev = prev.next
            # Move head forward
            head = head.next

        return dummy.next
```

