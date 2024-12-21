# [Leetcode 23: Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approaches
- [Naive Approach: Merge Lists Sequentially](#naive-approach-merge-lists-sequentially)
- [Using Min-Heap](#using-min-heap)
- [Divide and Conquer](#divide-and-conquer)

---

## Naive Approach: Merge Lists Sequentially

### Intuition:
The naive approach is to merge the lists one by one using a helper function that can merge two sorted linked lists. By doing this sequentially with all the k lists, we can obtain the final merged sorted linked list.

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def mergeTwoLists(l1, l2):
    dummy = ListNode(0)
    current = dummy
    # Merge two lists
    while l1 and l2:
        if l1.val < l2.val:
            current.next = l1
            l1 = l1.next
        else:
            current.next = l2
            l2 = l2.next
        current = current.next
    # if one list is finished, append the other list
    current.next = l1 if l1 else l2
    return dummy.next

def mergeKLists(lists):
    if not lists:
        return None
    merged_list = None
    for lst in lists:
        merged_list = mergeTwoLists(merged_list, lst)
    return merged_list
```

### Time Complexity:
- O(n * k), where n is the total number of nodes across all lists, as we are merging each list sequentially.
  
### Space Complexity:
- O(1), aside from the input and output lists, no additional space is used.

---

## Using Min-Heap

### Intuition:
A min-heap can efficiently give us the smallest current element across all lists. By placing the head of each list into the min-heap, we can always extract the smallest element, remove it from the heap, and add its next node to the heap.

```python
from heapq import heappush, heappop

class HeapNode:
    def __init__(self, node):
        self.node = node
    
    # Define comparison operators for the heap to use
    def __lt__(self, other):
        return self.node.val < other.node.val

def mergeKLists(lists):
    min_heap = []
    
    # Initialize a min heap with the head node of each list
    for l in lists:
        if l is not None:
            heappush(min_heap, HeapNode(l))
    
    dummy = ListNode(0)
    current = dummy

    while min_heap:
        # Pop the smallest element node
        min_node = heappop(min_heap).node
        
        # Add it to the result list
        current.next = min_node
        current = current.next
        
        # If the popped node has a next node, push it to the heap
        if min_node.next:
            heappush(min_heap, HeapNode(min_node.next))
            
    return dummy.next
```

### Time Complexity:
- O(n log k) where n is the total number of nodes, and k is the number of lists (because each insertion and extraction operation from the heap takes O(log k)).

### Space Complexity:
- O(k) for the heap.

---

## Divide and Conquer

### Intuition:
This approach leverages the idea of merging pairs of lists at a time reducing the number of lists being merged simultaneously through a divide and conquer strategy. This method utilizes less stack space compared to the naive pairwise merging.

```python
def mergeTwoLists(l1, l2):
    dummy = ListNode(0)
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

def mergeKLists(lists):
    if not lists:
        return None

    def merge_interval(start, end):
        if start == end:
            return lists[start]
        if start < end:
            mid = (start + end) // 2
            l1 = merge_interval(start, mid)
            l2 = merge_interval(mid + 1, end)
            return mergeTwoLists(l1, l2)
        return None
    
    return merge_interval(0, len(lists) - 1)
```

### Time Complexity:
- O(n log k), here each level of the division merges all n nodes. Since we effectively halve k on each merge step, there's O(log k) such levels.

### Space Complexity:
- O(log k), which is the space complexity for the recursion stack.

---

