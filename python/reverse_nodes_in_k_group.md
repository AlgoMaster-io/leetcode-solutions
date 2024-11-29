# [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approach: Iterative Solution with Dummy Node

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        if head is None or k == 1:
            return head

        dummy = ListNode(0)
        dummy.next = head
        prev_group = dummy

        while True:
            # Find the k-th node from prev_group
            kth_node = self.get_kth_node(prev_group, k)
            if kth_node is None:
                break  # Not enough nodes to reverse

            next_group = kth_node.next  # Node after the k-th node
            prev = next_group  # Start reversal with next_group as tail
            current = prev_group.next

            # Reverse k nodes
            while current != next_group:
                temp = current.next
                current.next = prev
                prev = current
                current = temp

            # Update the connections
            temp = prev_group.next
            prev_group.next = kth_node
            prev_group = temp

        return dummy.next

    def get_kth_node(self, start: ListNode, k: int) -> ListNode:
        while start is not None and k > 0:
            start = start.next
            k -= 1
        return start
```

