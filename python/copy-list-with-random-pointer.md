# [Leetcode 138: Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

## Approaches:
- [Approach 1: Using a HashMap (Dictionary)](#approach-1-using-a-hashmap-dictionary)
- [Approach 2: Using Interweaving Nodes](#approach-2-using-interweaving-nodes)

## Approach 1: Using a HashMap (Dictionary)

### Intuition
The given linked list has nodes with two types of pointers: a `next` pointer which points to the next node in the list and a `random` pointer which can point to any node in the list or be `null`. Our goal is to clone this complex linked list and ensure that the cloned list has correct random pointers.

A straightforward way to achieve this is by using a hashmap to map each original node to its corresponding copied node. First, we iterate through the original list and create a clone of each node. Then, we set the `next` and `random` pointers for each clone using the hashmap.

### Implementation
```python
class Node:
    def __init__(self, val: int, next: 'Node' = None, random: 'Node' = None):
        self.val = val
        self.next = next
        self.random = random

def copyRandomList(head: 'Optional[Node]') -> 'Optional[Node]':
    if not head:
        return None

    # A dictionary to map from original node to the cloned node.
    old_to_new = {}

    # Step 1: Iterate through the list and clone each node
    current = head
    while current:
        old_to_new[current] = Node(current.val)
        current = current.next

    # Step 2: Assign the next and random pointers for the cloned nodes
    current = head
    while current:
        if current.next:
            old_to_new[current].next = old_to_new[current.next]
        if current.random:
            old_to_new[current].random = old_to_new[current.random]
        current = current.next

    # Return the head of the cloned list.
    return old_to_new[head]
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the number of nodes in the linked list. We iterate through the list twice.
- **Space Complexity:** O(n), due to the hashmap which stores a mapping for each node.

## Approach 2: Using Interweaving Nodes

### Intuition
In this optimized approach, we aim to copy the list without using extra space for a hashmap. The trick here is to interleave cloned nodes with the original ones such that cloned node's `next` immediately follows its original. This way, the `random` pointer can be correctly assigned in a single pass.

### Implementation
```python
class Node:
    def __init__(self, val: int, next: 'Node' = None, random: 'Node' = None):
        self.val = val
        self.next = next
        self.random = random

def copyRandomList(head: 'Optional[Node]') -> 'Optional[Node]':
    if not head:
        return None

    # Step 1: Create a new interleaved list
    current = head
    while current:
        clone = Node(current.val, current.next)
        current.next = clone
        current = clone.next

    # Step 2: Assign random pointers to the cloned nodes
    current = head
    while current:
        if current.random:
            current.next.random = current.random.next
        current = current.next.next

    # Step 3: Restore the original list and extract the cloned list
    current = head
    pseudo_head = Node(0)
    clone_curr = pseudo_head

    while current:
        # Extract the cloned node
        clone = current.next
        clone_curr.next = clone
        clone_curr = clone

        # Restore the original list
        current.next = clone.next
        current = current.next

    return pseudo_head.next
```

### Complexity Analysis
- **Time Complexity:** O(n), since we iterate over the list multiple times but only a constant number of times.
- **Space Complexity:** O(1), as we don't use any additional space except for a few pointers.

The above code will help you seamlessly replicate the intricate structure of the original list with correct `random` pointers, ensuring optimal performance in terms of both time and space.

