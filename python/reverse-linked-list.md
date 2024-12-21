# [Leetcode 206: Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

## Approaches:
- [Iterative Approach](#iterative-approach)
- [Recursive Approach](#recursive-approach)

## Problem Statement:
Reverse a singly linked list.

---

### Iterative Approach

#### Intuition:
The iterative approach to reverse a linked list involves traversing the linked list and reversing the direction of each node's `next` pointer. We need three pointers to achieve this: `previous`, `current`, and `next_node`. `current` starts at the head node of the linked list. At each step, we'll store the `next_node` of `current`, point `current.next` to `previous`, move `previous` to `current`, and then move `current` to `next_node`.

#### Detailed Steps:
1. Initialize `previous` as `None` and `current` as `head`.
2. Iterate while `current` is not `None`:
   - Save the next node: `next_node = current.next`.
   - Reverse the `next` pointer: `current.next = previous`.
   - Move `previous` one step ahead: `previous = current`.
   - Move `current` one step ahead: `current = next_node`.
3. At the end of the iteration, `previous` will be pointing to the new head of the reversed list.

#### Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseList(head: ListNode) -> ListNode:
    previous = None
    current = head

    while current:
        next_node = current.next  # Save the next node
        current.next = previous   # Reverse the link
        previous = current        # Move `previous` pointer one step
        current = next_node       # Move `current` pointer one step

    return previous  # New head of the reversed list
```

#### Time Complexity: 
- **O(n)**: We traverse the list once, where `n` is the number of nodes in the list.
  
#### Space Complexity: 
- **O(1)**: No extra space except a few pointers.

---

### Recursive Approach

#### Intuition:
In the recursive approach, we recursively reverse the rest of the list from the second node onwards and then put the first element at the end. The base case will occur when the node is `None` (i.e., the end of the list) or `next` is `None` (i.e., every single element will become the new head eventually).

#### Detailed Steps:
1. If `head` is `None` or `head.next` is `None`, return `head` because the reverse of an empty or single node list is the list itself.
2. Recursively reverse the list starting from `head.next`.
3. Once the recursion starts to unwind, `head.next.next` is set to `head` to reverse the linkage, and then `head.next` is detached by setting it to `None`.
4. Return the new head returned by recursive calls.

#### Code:
```python
def reverseListRecursive(head: ListNode) -> ListNode:
    # If list is empty or has only one node
    if not head or not head.next:
        return head

    # Recursively reverse the rest of the list
    p = reverseListRecursive(head.next)
    head.next.next = head
    head.next = None

    return p  # New head of the reversed list
```

#### Time Complexity:
- **O(n)**: We traverse the list once recursively, where `n` is the number of nodes.

#### Space Complexity:
- **O(n)**: Stack space due to recursive calls, equivalent to the depth of recursion which is `n` in the worst case.

This solution performs the same operation as the iterative one but leverages the call stack for reversing links. Use an iterative approach in time-critical applications, but the recursive solution is often considered cleaner and more intuitive for mathematical settings.

