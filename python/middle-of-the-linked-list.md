# [Leetcode 876: Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approaches
1. [Two Pass Approach](#two-pass-approach)
2. [Two Pointer Approach (Floyd’s Cycle Finding Algorithm Inspired)](#two-pointer-approach)

---

## Two Pass Approach

### Intuition:
The intuitive approach to this problem is to make two passes through the linked list:
1. Count the total number of nodes in the linked list.
2. Traverse again to the middle node of the list using the node count from step 1.

By doing this, you can directly reach the middle node in a straightforward manner by using simple arithmetic.

### Steps:
1. Initialize two pointers, one that will traverse the list to count the nodes and another to find the middle node.
2. Traverse the entire linked list to count the total number of nodes (`n`).
3. Calculate the middle position: `middle = n // 2`.
4. Reset the head and start traversing again until we reach the middle position.
5. Return the node at the middle position.

### Code:
```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def middleNode(head: ListNode) -> ListNode:
    # First pointer to count the nodes
    current = head
    count = 0
    
    # Count the total number of nodes in the linked list
    while current:
        count += 1
        current = current.next
    
    # Calculate the index of the middle node
    middle = count // 2
    
    # Second pointer to traverse again to the middle
    current = head
    for _ in range(middle):
        current = current.next
    
    return current
```

### Time Complexity:
- **O(n)**: We make two full traversals of the list, which is linear with respect to the number of nodes `n`.

### Space Complexity:
- **O(1)**: We only use a few extra pointers and counters.

---

## Two Pointer Approach

### Intuition:
This approach is inspired by Floyd’s Cycle-Finding Algorithm commonly used for loop detection in linked lists. We use two pointers at different speeds:
1. A `slow` pointer that moves one step at a time.
2. A `fast` pointer that moves two steps at a time.

When the `fast` pointer reaches the end of the list, the `slow` pointer will have reached the middle.

### Steps:
1. Initialize `slow` and `fast` pointers at the head.
2. Move `slow` one step and `fast` two steps in a loop until `fast` reaches the end.
3. When `fast` reaches the end of the linked list, `slow` will be at the middle.

### Code:
```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def middleNode(head: ListNode) -> ListNode:
    slow = fast = head
    
    # Move slow by 1 step and fast by 2 steps
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
    # When loop breaks, slow will be at the middle node
    return slow
```

### Time Complexity:
- **O(n)**: The list is traversed at most a single time using the fast pointer, which dictates the speed of traversal.

### Space Complexity:
- **O(1)**: Only a small number of pointers are used regardless of the size of the input list. 

This approach is optimal in terms of time and space complexity.

