# 23. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approach 1: Priority Queue (Min-Heap)

### Solution
python
```python
# Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
# Space Complexity: O(k), for the priority queue
from queue import PriorityQueue

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists):
        # Min-heap to store the current nodes of each list
        min_heap = PriorityQueue()
        
        # Add the head of each non-empty list to the heap
        for node in lists:
            if node is not None:
                min_heap.put((node.val, node))
        
        dummy = ListNode(-1)  # Dummy node for result
        current = dummy

        while not min_heap.empty():
            # Get the smallest node from the heap
            smallest = min_heap.get()[1]
            current.next = smallest  # Append it to the result
            current = current.next

            # If the next node exists, add it to the heap
            if smallest.next is not None:
                min_heap.put((smallest.next.val, smallest.next))

        return dummy.next  # Return the merged list
```

## Approach 2: Divide and Conquer

### Solution
python
```python
# Time Complexity: O(n * log(k)), where n is the total number of elements and k is the number of lists
# Space Complexity: O(log(k)), due to recursive calls
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists):
        if not lists:
            return None
        return self.mergeLists(lists, 0, len(lists) - 1)

    def mergeLists(self, lists, left, right):
        if left == right:
            return lists[left]

        mid = left + (right - left) // 2
        l1 = self.mergeLists(lists, left, mid)
        l2 = self.mergeLists(lists, mid + 1, right)
        return self.mergeTwoLists(l1, l2)

    def mergeTwoLists(self, l1, l2):
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

        if l1:
            current.next = l1
        elif l2:
            current.next = l2

        return dummy.next
```


