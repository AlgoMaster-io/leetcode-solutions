# [Leetcode 82: Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## Approaches

- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Iterative with Dummy Node](#approach-2-iterative-with-dummy-node)

---

## Approach 1: Recursion

### Intuition

To solve this problem recursively, the main idea is to handle duplicates whenever they occur starting from the head of the list. We will recursively call to fix the rest of the list, then handle any duplicates at the current head.

### Steps

1. Base case: If the list is empty or contains only one node, it's already duplicate-free.
2. If the current node has a duplicate (its value is the same as the next node): skip all nodes with this value, then call the function recursively for the next node with a different value.
3. If there is no duplication at the start, proceed to the next node for processing.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def deleteDuplicates(head: ListNode) -> ListNode:
    if not head or not head.next:
        return head  # Base case: return the node if alone or null

    if head.val == head.next.val:
        # Skip nodes whose values are equal to head.
        while head.next and head.val == head.next.val:
            head = head.next  # Move head forward
        return deleteDuplicates(head.next)  # Process the rest of the list
    else:
        head.next = deleteDuplicates(head.next)  # Move the recursion to the next node
        return head  # Return the head since it's not a duplicate
```

### Complexity Analysis

- **Time Complexity**: O(n), where n is the number of nodes in the list since we only traverse the list once.
- **Space Complexity**: O(n), due to the recursive stack space.

---

## Approach 2: Iterative with Dummy Node

### Intuition

This iterative approach uses a dummy node to simplify edge cases where the head itself may be removed. The idea is to iterate over the list and use a "predecessor" to connect to unique nodes.

### Steps

1. Initialize a dummy node before the head. This helps easily return the new head.
2. Use two pointers: `prev` (to track the last node before the duplicates) and `current` to traverse the list.
3. Iterate while checking if the current node is a duplicate (check its subsequent nodes).
4. If duplicates are found, move `current` until the last duplicate and update `prev.next` to skip duplicates.
5. If no duplicates are found, move both `prev` and `current` forward.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def deleteDuplicates(head: ListNode) -> ListNode:
    dummy = ListNode(0)  # Create a dummy node
    dummy.next = head
    prev = dummy  # Predecessor before the current range of duplicates

    while head:
        # If it's at the start of duplicates sublist
        if head.next and head.val == head.next.val:
            # Move until the end of duplicates sublist
            while head.next and head.val == head.next.val:
                head = head.next
            # Skip all duplicates
            prev.next = head.next  # Bridge the predecessor to the node after duplicates
        else:
            # Move predecessor
            prev = prev.next
        # Move forward
        head = head.next
        
    return dummy.next  # dummy.next is the new head
```

### Complexity Analysis

- **Time Complexity**: O(n), where n is the number of nodes in the list since each node is visited at most twice.
- **Space Complexity**: O(1), since we only use a few additional pointers.

