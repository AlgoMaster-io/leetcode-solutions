# [61. Rotate List](https://leetcode.com/problems/rotate-list/)

## Approaches
- [Approach 1: Simulation](#approach-1-simulation)
- [Approach 2: Making the List Circular](#approach-2-making-the-list-circular)

## Approach 1: Simulation

**Intuition**:  
For the first approach, we can simulate the process of rotation. This involves repeatedly moving the last node to the front of the list until the desired number of rotations is achieved.

### Steps:
1. Calculate the length of the linked list.
2. If `k` is greater than the length, reduce it by taking `k % length`. This is because rotating the list by its length results in the same list.
3. Break the list at the `n-k` position and reattach the end to the front.

### Implementation:

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def rotateRight(head: ListNode, k: int) -> ListNode:
    if not head or not head.next or k == 0:
        return head
    
    # Find the length of the linked list
    length = 1
    tail = head
    while tail.next:
        tail = tail.next
        length += 1
        
    # To handle cases where k >= length
    k = k % length
    
    if k == 0:
        return head
    
    # Find the new tail, which will be at length - k - 1 position
    new_tail = head
    for _ in range(length - k - 1):
        new_tail = new_tail.next
    
    # The new head will be the next of the new tail
    new_head = new_tail.next
    new_tail.next = None  # Break the link
    
    # Connect the old tail to the old head
    tail.next = head
    
    return new_head
```

**Time Complexity**: O(N), where N is the length of the list.  
**Space Complexity**: O(1), as we are changing the nodes' links in-place.

## Approach 2: Making the List Circular

**Intuition**:  
For an optimized approach, consider making the list circular. We can connect the end of the list to the head and then break the circle at the appropriate position. This way, most operations are reduced to a single pass over the list.

### Steps:
1. Find the length of the linked list and connect the last node to the first node, forming a circle.
2. Calculate the effective number of rotations needed using `k % length`.
3. Determine the new tail, which is the node at `length - k` position.
4. Determine the new head as `new_tail.next`.
5. Break the circle by setting `new_tail.next = None`.

### Implementation:

```python
def rotateRight(head: ListNode, k: int) -> ListNode:
    if not head or not head.next or k == 0:
        return head
    
    # Find the length of the list
    length = 1
    tail = head
    while tail.next:
        tail = tail.next
        length += 1

    # Make the linked list circular
    tail.next = head

    # Calculate the actual rotation steps needed
    k = k % length
    
    # Find the new tail position
    steps_to_new_tail = length - k
    new_tail = head
    for _ in range(steps_to_new_tail - 1):
        new_tail = new_tail.next
    
    # The new head is next to the new tail
    new_head = new_tail.next
    
    # Break the circle
    new_tail.next = None
    
    return new_head
```

**Time Complexity**: O(N), where N is the length of the list.  
**Space Complexity**: O(1), as we manipulate the list in-place without extra data structures.

