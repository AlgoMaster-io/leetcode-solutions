# [Leetcode 142: Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approaches:
1. [HashSet Approach](#hashset-approach)
2. [Two Pointers (Floyd's Cycle Detection Algorithm)](#two-pointers-floyds-cycle-detection-algorithm)

---

### HashSet Approach

**Intuition:**  
The simplest way to detect if there is a cycle in a linked list is to use extra space to store nodes we've already visited. By storing each node in a `set`, we can check for the existence of a cycle by looking if a node has already been seen. If a node is visited twice, there's a cycle. For the start of the cycle, the node where this repetition first occurs is the start of the cycle.

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def detectCycle(head: ListNode) -> ListNode:
    visited_nodes = set()
    current = head
    
    while current:
        # If we have already seen this node, there's a cycle and this is the start
        if current in visited_nodes:
            return current
        
        # Mark this node as visited
        visited_nodes.add(current)
        
        # Move to the next node
        current = current.next
    
    # Return None if there is no cycle
    return None
```

**Time Complexity:** O(n), where n is the number of nodes in the linked list. We may visit each node at most once.  
**Space Complexity:** O(n), due to the storage used to record visited nodes.

---

### Two Pointers (Floyd's Cycle Detection Algorithm)

**Intuition:**  
Floyd's Cycle Detection algorithm uses two pointers that move at different speeds (slow and fast). The fast pointer moves two steps at a time while the slow pointer moves one step. If there's a cycle, the two pointers will eventually meet inside the cycle.

To find the entrance, after the initial meeting point, we set one pointer to the head of the list and the other remains at the meeting point. Move both one step at a time. Where they meet again, is the start of the cycle.

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def detectCycle(head: ListNode) -> ListNode:
    if not head or not head.next:
        return None
    
    # Initialize two pointers
    slow, fast = head, head
    
    while fast and fast.next:
        slow = slow.next       # Move slow pointer by 1 step
        fast = fast.next.next  # Move fast pointer by 2 steps
        
        if slow == fast:       # Cycle is detected
            break
    
    # If no cycle
    if not fast or not fast.next:
        return None
    
    # Set one pointer back to head, other remains at meeting point
    slow = head
    while slow != fast:       # Move both at the same speed
        slow = slow.next
        fast = fast.next
    
    # Return the start of the cycle
    return slow
```

**Time Complexity:** O(n), where n is the number of nodes in the linked list. In the worst case, we will traverse every node once outside the cycle and at most twice inside the cycle.  
**Space Complexity:** O(1), as no extra space is used beyond the two pointers.

