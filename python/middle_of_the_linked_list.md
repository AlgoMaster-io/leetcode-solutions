# [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approach 1: Two Passes (Count Nodes)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def middleNode(self, head: ListNode) -> ListNode:
        count = 0
        current = head

        # Count the total number of nodes
        while current:
            count += 1
            current = current.next

        # Find the middle index
        middle_index = count // 2
        current = head

        # Move to the middle node
        for _ in range(middle_index):
            current = current.next

        return current
```

## Approach 2: Fast and Slow Pointers (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def middleNode(self, head: ListNode) -> ListNode:
        slow = head
        fast = head

        # Move fast pointer twice as fast as the slow pointer
        while fast and fast.next:
            slow = slow.next  # Move slow pointer one step
            fast = fast.next.next  # Move fast pointer two steps

        return slow  # Slow pointer will be at the middle
```

