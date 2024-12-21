# [Leetcode Problem 92: Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

## Approaches:
- [Simple Iterative Approach](#simple-iterative-approach)
- [Optimized One Pass Approach](#optimized-one-pass-approach)

### Simple Iterative Approach

**Intuition:**

The problem requires reversing a part of a linked list from position `m` to `n`. The simplest approach is to traverse the linked list to the `m-1` position, then reverse the sublist from `m` to `n`. Finally, connect the reversed sublist back to the original list.

**Steps:**

1. Traverse the linked list to reach the node just before the `m-th` position (`pre` node).
2. Use three pointers (`pre`, `reverse`, and `current`). 
   - `pre` is the node just before reversal.
   - `current` is initially the `m-th` node where the reversal begins.
   - `reverse` holds the reversed portion of the sublist.
3. Traverse from `m` to `n` nodes, reversing the sublist in-place.
4. Finally, reconnect the reversed sublist back to the main list.

**Code:**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseBetween(head: ListNode, left: int, right: int) -> ListNode:
    if not head or left == right:
        return head

    dummy = ListNode(0)
    dummy.next = head
    pre = dummy

    # Move `pre` to the node before the `left-th` node
    for _ in range(left - 1):
        pre = pre.next

    # Start reversing from `left` to `right`
    reverse = None
    current = pre.next
    for _ in range(right - left + 1):
        next_node = current.next  # Store next node
        current.next = reverse  # Reverse the current node
        reverse = current  # Move reverse to current
        current = next_node  # Move current to the next node

    # Connect `pre.next` to the reversed nodes
    pre.next.next = current  # Connect last node of reversed sublist to the `right+1` node
    pre.next = reverse  # Connect `pre` to the new head of the reversed sublist

    return dummy.next
```

**Time Complexity:** O(N) - We traverse the list once.  
**Space Complexity:** O(1) - In-place reversal using a constant amount of extra space.

### Optimized One Pass Approach

**Intuition:**

The one-pass solution aims to do the reversal in a single traversal of the list as the above solution. We keep track of nodes involved in the reversal process in one pass using all three pointers as directly used above. The intention is to achieve in-place without any re-arrangement outside the sublist.

**Code:**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseBetween(head: ListNode, left: int, right: int) -> ListNode:
    if not head or left == right:
        return head

    dummy = ListNode(0)
    dummy.next = head
    pre = dummy

    # Move `pre` to the node before the `left-th` node
    for _ in range(left - 1):
        pre = pre.next

    # Reverse the segment
    current = pre.next
    for _ in range(right - left):
        temp = current.next
        current.next = temp.next
        temp.next = pre.next
        pre.next = temp

    return dummy.next
```

**Time Complexity:** O(N) - We traverse the list once in a single pass.  
**Space Complexity:** O(1) - Reversal is done in-place without extra storage.

These approaches cover the iterative in-place reversal with clear details and complexity analysis, demonstrating the efficiency improvements as we move to a unique pointer manipulation method.

