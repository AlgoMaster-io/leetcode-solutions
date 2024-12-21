# [Leetcode Problem 24: Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

## Approaches
- [Approach 1: Iterative Solution Using Swapping](#approach-1-iterative-solution-using-swapping)
- [Approach 2: Recursive Solution](#approach-2-recursive-solution)

## Approach 1: Iterative Solution Using Swapping

**Intuition:**
- The problem asks us to swap every two adjacent nodes in a linked list. We will use an iterative approach for a simple and straightforward solution.
- We'll iterate through the list and swap each pair of nodes by manipulating pointers.

### Code Implementation:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def swapPairs(head: ListNode) -> ListNode:
    # Dummy node acts as the starting point to simplify edge cases
    dummy = ListNode(0)
    dummy.next = head
    current = dummy

    # Iterate through the list while there are at least two nodes to swap
    while current.next and current.next.next:
        # Identify the nodes to swap
        first = current.next
        second = current.next.next
        
        # Swap the nodes
        # First's next should point to what Second was pointing to
        first.next = second.next
        # Second's next should point to First
        second.next = first
        # Current's next should now point to Second
        current.next = second

        # Move 'current' two nodes forward in the list
        current = first
        
    return dummy.next
```

### Complexity Analysis:
- **Time Complexity:** O(n), where n is the number of nodes in the linked list; we traverse the list once.
- **Space Complexity:** O(1), as we do not use any extra space that scales with input size.

## Approach 2: Recursive Solution

**Intuition:**
- A recursive approach achieves the node swap using recursion to handle the pair-wise swaps as a series of sub-solutions.
- Swap the first two nodes and recursively call for the rest of the list.

### Code Implementation:
```python
def swapPairsRecursive(head: ListNode) -> ListNode:
    # Base case: if there's only one node or no node, return the head as is
    if not head or not head.next:
        return head
    
    # Nodes to swap
    first = head
    second = head.next

    # Recursively call for the rest of the list beyond the second node
    first.next = swapPairsRecursive(second.next)
    # Swap first and second
    second.next = first

    # Return the new "head" node of the current pair
    return second
```

### Complexity Analysis:
- **Time Complexity:** O(n), where n is the number of nodes in the linked list; every recursive call processes two nodes.
- **Space Complexity:** O(n), due to the recursion stack used in processing recursive calls.

Both methods efficiently solve the problem, with the iterative method being slightly more preferable in terms of space complexity. Choose the method that fits your preference for solving problems using iteration or recursion.

