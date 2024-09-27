
# Reverse Linked List II
Given the head of a singly linked list and two integers left and right where 1 <= left <= right <= n, reverse the nodes of the list from position left to position right, and return the reversed list.

### Constraints:
- The number of nodes in the list is n.
- 1 <= n <= 500.
- -500 <= Node.val <= 500.
- 1 <= left <= right <= n.

### Examples
```javascript
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]

Input: head = [5], left = 1, right = 1
Output: [5]
```

### Follow-up:
A linked list can be reversed either iteratively or recursively. Could you implement both?

## Approaches to Solve the Problem
### Approach 1: Iterative Reversal with Dummy Node (Optimal Solution)
##### Intuition:
The problem requires reversing a sublist within a linked list while keeping the rest of the list intact. To achieve this:

1. Traverse the list until you reach the node just before the left position.
2. Reverse the sublist between left and right.
Reconnect the reversed sublist to the rest of the list.
3. A dummy node can help simplify edge cases, such as when the sublist to reverse starts at the head of the list.

Steps:
1. Dummy Node: Create a dummy node before the head to handle edge cases like when left is at the head.
2. Traverse the List: Move a pointer to the node just before left to identify where to start reversing.
3. Reverse the Sublist: Reverse the nodes between positions left and right.
4. Reconnection: Reconnect the reversed sublist back to the rest of the list.
##### Visualization:
For head = [1, 2, 3, 4, 5], left = 2, and right = 4:
```rust
Initial list:
1 -> 2 -> 3 -> 4 -> 5

After reversing the sublist between 2 and 4:
1 -> 4 -> 3 -> 2 -> 5
```
##### Time Complexity:
O(n), where n is the number of nodes in the list. We traverse the list once to reverse the sublist.
##### Space Complexity:
O(1), as we only use a few pointers to modify the list in place.
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseBetween(head: ListNode, left: int, right: int) -> ListNode:
    # Step 1: Create a dummy node before the head
    dummy = ListNode(0)
    dummy.next = head
    prev = dummy
    
    # Step 2: Move prev to the node just before left
    for _ in range(left - 1):
        prev = prev.next
    
    # Step 3: Start the reversal process
    curr = prev.next
    next_node = None
    
    for _ in range(right - left):
        next_node = curr.next
        curr.next = next_node.next
        next_node.next = prev.next
        prev.next = next_node
    
    return dummy.next
```
##### Breakdown of the Code:
1. Dummy Node Creation: A dummy node simplifies the edge case where the reversal starts from the head.
2. Traversing to Left: The pointer prev is moved to the node just before left. This is where the reversal begins.
3. Reversing the Sublist: Using the next pointer of each node, the sublist is reversed in place.
4. Reconnection: Once the sublist is reversed, the nodes are reconnected to the rest of the list.
5. Return: The modified list is returned by pointing to dummy.next.

### Approach 2: Recursive Solution
##### Intuition: 
A recursive approach could be employed by focusing on reversing a portion of the list recursively. While this approach is less efficient than the iterative method in terms of space, it's conceptually simpler.

However, recursive approaches for in-place linked list reversal are rarely used in practice for reversing specific sublists due to complexity in handling edge cases and performance concerns related to recursion depth.

Steps:
1. Identify the base case when the left and right positions are met.
2. Reverse the sublist recursively by adjusting the pointers during the unwinding phase.
3. Return the modified head after the recursion completes.
##### Time Complexity:
O(n), where n is the number of nodes in the list.
##### Space Complexity:
O(n), due to the recursion stack.
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseBetween(head: ListNode, left: int, right: int) -> ListNode:
    if left == right:
        return head

    def reverseN(head, n):
        if n == 1:
            return head, head.next
        new_head, successor = reverseN(head.next, n - 1)
        head.next.next = head
        head.next = successor
        return new_head, successor

    if left == 1:
        new_head, _ = reverseN(head, right)
        return new_head

    head.next = reverseBetween(head.next, left - 1, right - 1)
    return head
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Iterative                    | O(n)      | O(1)             |
| Recursive                          | O(n)            | O(n)             |

The iterative solution with dummy node is the most efficient and practical approach due to its O(1) space complexity and straightforward handling of edge cases. The recursive solution is elegant but less efficient because of the recursion depth and overhead.