# [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Approach: Iterative Solution with Carry

### Solution
python
```python
# Time Complexity: O(max(m, n)), where m and n are the lengths of the two lists
# Space Complexity: O(max(m, n)), for the new linked list

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode(0)  # Dummy node to simplify the process
        current = dummy  # Pointer to the current node in the result list
        carry = 0  # Store carry from the addition

        # Traverse both lists
        while l1 is not None or l2 is not None or carry != 0:
            sum = carry  # Start with the carry

            # Add values from l1 and l2 if they exist
            if l1 is not None:
                sum += l1.val
                l1 = l1.next
            if l2 is not None:
                sum += l2.val
                l2 = l2.next

            # Calculate new carry and the digit to store
            carry = sum // 10
            current.next = ListNode(sum % 10)  # Store the last digit of the sum
            current = current.next  # Move to the next node

        return dummy.next  # Skip the dummy node
```

