# [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

## Approach 1: Iterative Solution

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head: 'ListNode') -> 'ListNode':
        prev = None  # Previous node, initially None
        current = head  # Current node to process

        while current is not None:
            next_temp = current.next  # Store the next node
            current.next = prev  # Reverse the link
            prev = current  # Move prev to the current node
            current = next_temp  # Move to the next node

        return prev  # New head of the reversed list
```

## Approach 2: Recursive Solution

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n) (due to recursion stack)

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head: 'ListNode') -> 'ListNode':
        # Base case: if the list is empty or has only one node
        if head is None or head.next is None:
            return head

        # Reverse the rest of the list
        reversed_list = self.reverseList(head.next)

        # Reverse the current node
        head.next.next = head
        head.next = None

        return reversed_list  # Return the new head
```

