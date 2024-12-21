# [Leetcode Problem 21: Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

## Solutions

- [Solution 1: Iterative Approach](#solution-1-iterative-approach)
- [Solution 2: Recursive Approach](#solution-2-recursive-approach)

### Solution 1: Iterative Approach

This solution involves iteratively merging two linked lists by comparing each node one by one. We create a new linked list, which we build in sorted order.

#### Intuition

- Initialize a dummy node to use as the starting anchor for the new merged list.
- Use a `current` pointer to keep track of the last node in the merged list.
- Compare the nodes from both lists: append the smaller node to the merged list, and advance the pointer on the list from which the node was taken.
- Repeat until one of the lists is exhausted, and then append the remaining nodes from the other list to the merged list.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def mergeTwoLists(l1: ListNode, l2: ListNode) -> ListNode:
    # Create a dummy node to make edge case handling easier
    dummy = ListNode()
    # This pointer will be used to build the new linked list
    current = dummy
    
    # Loop until either list is exhausted
    while l1 and l2:
        if l1.val < l2.val:
            # l1's node is smaller, append it to the new list
            current.next = l1
            l1 = l1.next
        else:
            # l2's node is smaller or equal, append it to the new list
            current.next = l2
            l2 = l2.next
        # Move the current pointer to the new end of the list
        current = current.next
    
    # At least one of the lists is exhausted, append the remaining list to the merged one
    current.next = l1 if l1 else l2
    
    # The merged list starts at dummy.next
    return dummy.next
```

**Time Complexity**: `O(n + m)` where `n` and `m` are the lengths of the two lists. We go through each list once.

**Space Complexity**: `O(1)` because we only use a fixed amount of extra space.

### Solution 2: Recursive Approach

This approach uses recursion to merge the two linked lists by selecting the smaller node as the head and continuing the process for the rest of the lists.

#### Intuition

- Base case: If either list is null, return the other list because it represents the part of the merged list that remains to be constructed.
- Recursive case: Compare the heads of both lists. If one is smaller, set it as the current node and recursively merge the rest of the lists.
- The smaller node's `next` will point to the result of the recursive merge of the remaining nodes.

```python
def mergeTwoLists(l1: ListNode, l2: ListNode) -> ListNode:
    # Base cases: when either list is empty, return the other list
    if not l1:
        return l2
    elif not l2:
        return l1
    
    # Compare the values at the head of l1 and l2
    if l1.val < l2.val:
        # If l1's node is smaller, it should be part of the merged list
        l1.next = mergeTwoLists(l1.next, l2)
        return l1
    else:
        # If l2's node is smaller or equal, it should be part of the merged list
        l2.next = mergeTwoLists(l1, l2.next)
        return l2
```

**Time Complexity**: `O(n + m)` where `n` and `m` are the lengths of the two lists. Each node is processed exactly once.

**Space Complexity**: `O(n + m)` due to the recursion stack. Each function call adds a new layer to the stack.

