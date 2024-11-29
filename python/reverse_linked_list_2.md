# [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

## Approach: Iterative Solution with Dummy Node

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def reverseBetween(self, head: ListNode, left: int, right: int) -> ListNode:
        if not head or left == right:
            return head

        dummy = ListNode(0) # Dummy node to simplify edge cases
        dummy.next = head
        prev = dummy

        # Step 1: Move prev to the node before the "left" position
        for _ in range(left - 1):
            prev = prev.next

        # Step 2: Reverse the sublist from "left" to "right"
        current = prev.next # The first node to be reversed
        next_node = None # Temporary node to store the next node
        prev_sublist = None # Tracks the reversed sublist

        for _ in range(right - left + 1):
            next_node = current.next # Store the next node
            current.next = prev_sublist # Reverse the current node
            prev_sublist = current # Move prev_sublist to the current node
            current = next_node # Move to the next node

        # Step 3: Reconnect the reversed sublist with the rest of the list
        prev.next.next = current # Connect the tail of the reversed sublist
        prev.next = prev_sublist # Connect the head of the reversed sublist

        return dummy.next # Return the updated list
```


