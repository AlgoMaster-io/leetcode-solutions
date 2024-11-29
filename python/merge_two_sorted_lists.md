# [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

## Approach 1: Iterative

### Solution
python
```python
# Time Complexity: O(n + m), where n and m are the lengths of the two lists
# Space Complexity: O(1)

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeTwoLists(self, list1, list2):
        dummy = ListNode(-1)  # Dummy node to simplify edge cases
        current = dummy

        while list1 is not None and list2 is not None:
            if list1.val <= list2.val:
                current.next = list1  # Append list1's current node
                list1 = list1.next  # Move list1 to the next node
            else:
                current.next = list2  # Append list2's current node
                list2 = list2.next  # Move list2 to the next node
            current = current.next  # Move the pointer forward

        # Append the remaining nodes from either list
        if list1 is not None:
            current.next = list1
        else:
            current.next = list2

        return dummy.next  # Return the merged list starting at dummy.next
```

## Approach 2: Recursive

### Solution
python
```python
# Time Complexity: O(n + m), where n and m are the lengths of the two lists
# Space Complexity: O(n + m) (due to recursion stack)

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeTwoLists(self, list1, list2):
        if list1 is None:
            return list2  # If list1 is empty, return list2
        if list2 is None:
            return list1  # If list2 is empty, return list1

        # Merge the smaller node with the result of the recursive call
        if list1.val <= list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2
```

