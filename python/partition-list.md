# [Leetcode 86: Partition List](https://leetcode.com/problems/partition-list/)

## Approaches
- [Approach 1: Two separate lists (Basic approach)](#approach-1-two-separate-lists-basic-approach)
- [Approach 2: Two-pointer with optimized space](#approach-2-two-pointer-with-optimized-space)

---

## Approach 1: Two separate lists (Basic approach)

### Intuition
The goal of this problem is to reorder the given linked list so that all nodes less than a given value `x` appear before nodes greater than or equal to `x`. To achieve this, we can split the list into two separate lists: one for nodes with values less than `x` and another for nodes with values greater than or equal to `x`. Once these two lists are constructed, we can combine them to form the desired reordered list.

### Steps
1. Create two new linked lists: `less_head` and `greater_head`, which serve as dummy heads for the lists of nodes less than `x` and nodes greater or equal to `x`, respectively.
2. Traverse the original list. For each node:
   - If its value is less than `x`, append it to the `less_head` list.
   - Otherwise, append it to the `greater_head` list.
3. After the traversal, link the end of the `less_head` list to the beginning of the `greater_head` list.
4. Return the head of the combined list, which is `less_head.next`.

### Time Complexity
- The algorithm traverses the list once, so the time complexity is O(n), where n is the number of nodes in the linked list.

### Space Complexity
- The algorithm uses additional space for the two new linked lists, resulting in O(n) space complexity.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def partition(head: ListNode, x: int) -> ListNode:
    # Create two dummy heads for the less and greater lists
    less_head = ListNode(0)
    greater_head = ListNode(0)
    
    # Pointers to build the new lists
    less = less_head
    greater = greater_head
    
    # Traverse the original list
    while head:
        if head.val < x:
            # Attach node to the end of the less list
            less.next = head
            less = less.next
        else:
            # Attach node to the end of the greater list
            greater.next = head
            greater = greater.next
        
        # Move to the next node in the original list
        head = head.next
    
    # Connect the less list with the greater list
    greater.next = None  # Important! Otherwise, a cycle could be created
    less.next = greater_head.next
    
    # Return the head of the newly partitioned list
    return less_head.next
```

---

## Approach 2: Two-pointer with optimized space

### Intuition
This approach seeks to reduce space usage and pointer manipulation. We maintain two pointers in the existing list to identify parts of the list that need to be moved. Instead of creating new nodes, we rearrange the nodes using a two-pointer technique within the same list.

### Steps
1. Initialize two pointers, `left` for the last known position of a node less than `x`, and a scanner (`current`) that scans every node in the list.
2. Traverse through the list:
   - If a node’s value is less than `x`, swap it with the node at the `left` position and move both pointers forward.
   - If the node’s value is not less than `x`, just move the `current` pointer forward.
3. Continue until every node has been processed.

### Time Complexity
- As we are processing all elements once, the time complexity remains O(n).

### Space Complexity
- The space complexity is reduced to O(1), as no extra space is used apart from a few variables.

```python
def partition(head: ListNode, x: int) -> ListNode:
    # Pointers to new heads for less and greater lists
    less_head = ListNode(0)
    greater_head = ListNode(0)
    
    # Tails to track ends of the less and greater list
    less_tail = less_head
    greater_tail = greater_head
    
    current = head
    while current:
        next_node = current.next  # Save the next node
        if current.val < x:
            less_tail.next = current
            less_tail = current
        else:
            greater_tail.next = current
            greater_tail = current
        current.next = None  # Disconnect current node's next to maintain two separate lists
        current = next_node  # Move to the saved next node
    
    # Connect the less part with the greater part
    less_tail.next = greater_head.next
    
    # Return the dummy head's next of the less list which is the start of the merged list
    return less_head.next
``` 

Each approach has its merits, with the second one carefully optimizing space by not allocating additional nodes, and both achieving an O(n) traverse complexity.

