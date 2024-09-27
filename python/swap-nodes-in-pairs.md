
# Swap Nodes in Pairs
Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed).

### Constraints:
- The number of nodes in the list is in the range [0, 100].
- 0 <= Node.val <= 100.

### Examples
```javascript
Input: head = [1,2,3,4]
Output: [2,1,4,3]

Input: head = []
Output: []

Input: head = [1]
Output: [1]
```

## Approaches to Solve the Problem
### Approach 1: Iterative Approach with Dummy Node (Optimal)
##### Intuition:
The iterative approach uses a dummy node to simplify handling edge cases, such as when swapping the first two nodes. We traverse the list and swap adjacent nodes in pairs by adjusting pointers. This method ensures we don’t modify the node values, but rather rearrange the actual nodes.

- A dummy node simplifies edge cases when swapping the head node, allowing consistent pointer manipulations.
- We maintain three pointers (prev, first, and second) to swap each pair of nodes.

Steps:
1. Create a dummy node that points to the head of the list.
2. Set prev as the dummy node and start a loop where you check pairs of nodes (first and second).
3. For each pair:
   - Point prev.next to second.
   - Swap the pointers between first and second so that they are reversed.
   - Move the prev pointer to first (which is now after second) and continue.
   - Return dummy.next as the new head of the list.
##### Visualization:
For head = [1, 2, 3, 4]:

Initial list:
1 -> 2 -> 3 -> 4

After swapping the first pair (1 and 2):
dummy -> 2 -> 1 -> 3 -> 4

After swapping the next pair (3 and 4):
dummy -> 2 -> 1 -> 4 -> 3
```
##### Time Complexity:
O(n), where n is the number of nodes in the list. We traverse the list once, swapping pairs of nodes.
##### Space Complexity:
O(1), as we only use a few pointers to rearrange the nodes.
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def swapPairs(head: ListNode) -> ListNode:
    # Step 1: Create a dummy node
    dummy = ListNode(0)
    dummy.next = head
    prev = dummy
    
    # Step 2: Traverse the list and swap pairs
    while head and head.next:
        first = head
        second = head.next
        
        # Step 3: Swap the nodes
        prev.next = second
        first.next = second.next
        second.next = first
        
        # Move to the next pair
        prev = first
        head = first.next
    
    return dummy.next
```

### Approach 2: Recursive Solution
##### Intuition: 
The recursive solution works by treating the problem as smaller subproblems. For each pair of nodes, swap them, and then recursively call the function to swap the rest of the list. This breaks the problem down into smaller parts until there are no more pairs to swap.

Base case: If there are fewer than two nodes, no swapping is necessary.
Recursive case: Swap the first two nodes and then recursively swap the remaining list.

Steps:
1. Base case: If head is None or head.next is None, return head.
2. Swap the first two nodes (head and head.next).
3. Recursively call the function for the rest of the list starting from head.next.next.
4. Connect the result of the recursive call to head.next.
### Visualization
For head = [1, 2, 3, 4]:
```rust
Recursive call: swapPairs([3, 4]) → returns [4, 3]
Swap first two nodes in the current call:
1 -> 2 → becomes 2 -> 1
Connect to [4, 3]:
2 -> 1 -> 4 -> 3
```
##### Time Complexity:
O(n), where n is the number of nodes in the list. Each node is visited once.
##### Space Complexity:
O(n), due to the recursion stack.
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def swapPairs(head: ListNode) -> ListNode:
    # Base case: if there are fewer than 2 nodes, return head
    if not head or not head.next:
        return head
    
    # Step 1: Swap the first two nodes
    first = head
    second = head.next
    
    # Recursively call for the rest of the list
    first.next = swapPairs(second.next)
    
    # Point second to first to complete the swap
    second.next = first
    
    return second  # second is the new head of the swapped pair
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Iterative                    | O(n)      | O(1)             |
| Recursive                          | O(n)            | O(n)             |

The iterative solution with a dummy node is generally preferred because of its constant space complexity and clear handling of the list. The recursive solution is elegant but has a higher space complexity due to the recursion stack.