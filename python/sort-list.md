# [148. Sort List](https://leetcode.com/problems/sort-list/)

## Approaches

1. [Merge Sort using Recursion](#merge-sort-using-recursion)
2. [Merge Sort with Bottom-Up Iteration](#merge-sort-with-bottom-up-iteration)

## Merge Sort using Recursion

### Intuition

The problem requires sorting a linked list in O(n log n) time and O(1) space (for the merge sort comparison operation). A classic algorithm for this is merge sort, which inherently works well with linked list structures due to ease of splitting and merging. 

- **Splitting:** Find the middle of the linked list using a slow and fast pointer approach. The fast pointer moves two steps for every step the slow pointer takes. When the fast pointer reaches the end, the slow pointer will be at the middle.
- **Merging:** Merge two sorted halves. This is easily done by comparing node values and adjusting pointers.

### Detailed Steps

1. If the list is empty or has only one node, it is already sorted - return.
2. Use a two-pointer technique to find the list's middle and split it into two halves.
3. Recursively sort each half.
4. Merge the sorted halves.

### Code

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def sortList(head: ListNode) -> ListNode:
    # Base case: If the list is empty or has only one element, return it.
    if not head or not head.next:
        return head
    
    # Step 1: Split the linked list into two halves.
    def get_mid(head):
        slow, fast = head, head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
    
    mid = get_mid(head)
    right = mid.next
    mid.next = None
    
    # Step 2: Recursively sort the split halves.
    left_sorted = sortList(head)
    right_sorted = sortList(right)
    
    # Step 3: Merge the two sorted halves.
    def merge(left, right):
        dummy = ListNode()
        tail = dummy
        while left and right:
            if left.val < right.val:
                tail.next = left
                left = left.next
            else:
                tail.next = right
                right = right.next
            tail = tail.next
        tail.next = left if left else right
        return dummy.next
    
    return merge(left_sorted, right_sorted)
```

### Time Complexity

- **Splitting:** O(log n), each level of recursion is halving the list.
- **Merging:** O(n) at each level with n/2 nodes.
- Combining them gives O(n log n).

### Space Complexity

- Recursive stack space is O(log n) due to divide and conquer.
- Merging the lists uses O(1) space beyond input data, satisfying the problem constraints.


## Merge Sort with Bottom-Up Iteration

### Intuition

An iterative approach that mimics the sort with the usage of pointers instead of recursion. This allows us to be space-efficient and bypass the recursion stack. Instead of dividing top-down, we start merging bottom-up.

- **Iterative Merging:** Merge sublists of increasing length: 1, 2, 4, 8, ... until the whole list is sorted.

### Detailed Steps

1. Determine the length of the linked list.
2. Starting with sublists of length 1, merge pairs of sublists, doubling the length of sublists each cycle.
3. Continue until the entire list is a single sorted segment.

### Code

```python
def sortList(head: ListNode) -> ListNode:
    if not head or not head.next:
        return head

    def get_length(head):
        length = 0
        while head:
            head = head.next
            length += 1
        return length
    
    def merge(l1, l2):
        dummy = ListNode()
        tail = dummy
        while l1 and l2:
            if l1.val < l2.val:
                tail.next = l1
                l1 = l1.next
            else:
                tail.next = l2
                l2 = l2.next
            tail = tail.next
        tail.next = l1 if l1 else l2
        return dummy.next

    length = get_length(head)
    step = 1
    dummy = ListNode(0, head)
    
    while step < length:
        tail = dummy
        current = dummy.next
        while current:
            left = current
            right = split(left, step)
            current = split(right, step)
            tail.next = merge(left, right)
            while tail.next:
                tail = tail.next
        step *= 2
        
    return dummy.next

def split(node, step):
    for i in range(step - 1):
        if node:
            node = node.next
    if not node:
        return None
    next_part, node.next = node.next, None
    return next_part
```

### Time Complexity

- Determines the length, O(n).
- Iterative merging has O(n log n) complexity.

### Space Complexity

- Merging is done in-place and hence has O(1) space complexity, adhering to problem constraints.

