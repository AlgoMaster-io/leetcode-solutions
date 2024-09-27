
# Reverse Linked List
Given the head of a singly linked list, reverse the list and return the reversed list.

### Constraints:
- The number of nodes in the list is the range [0, 5000].
- -5000 <= Node.val <= 5000

### Examples
```javascript
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]

Input: head = [1,2]
Output: [2,1]

Input: head = []
Output: []
```

### Follow-up:
A linked list can be reversed either iteratively or recursively. Could you implement both?

## Approaches to Solve the Problem
### Approach 1: Iterative Solution (Optimal)
##### Intuition:
In the iterative approach, the goal is to reverse the direction of the links between the nodes. We can achieve this by maintaining three pointers: prev, curr, and next. We iterate through the list, reversing the next pointer of each node until we reach the end of the list.

- prev: Keeps track of the previous node (initially None because the new tail will point to None).
- curr: Points to the current node being processed.
- next: Points to the next node in the original list so that we can continue processing after modifying the current node’s next pointer.

Steps:
1. Initialize three pointers: prev = None, curr = head, and next = None.
2. While curr is not None, repeat the following steps:
   - Store curr.next in next to keep track of the remaining list.
   - Set curr.next to prev to reverse the link.
   - Move prev to curr and curr to next.
3. After the loop, prev will point to the new head of the reversed list.
4. Return prev.
##### Visualization:
For head = [1, 2, 3, 4, 5]:
```rust
Initial:
prev = None, curr = 1, next = None

Iteration 1:
next = 2
Reverse 1's next to None → prev = 1, curr = 2

Iteration 2:
next = 3
Reverse 2's next to 1 → prev = 2, curr = 3

...

Final:
prev = 5 (new head), curr = None
Reversed list: [5, 4, 3, 2, 1]
```
##### Time Complexity:
O(n), where n is the number of nodes in the list. We traverse the list once.
##### Space Complexity:
O(1), as we only use a few pointers to reverse the list in place.
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseList(head: ListNode) -> ListNode:
    prev = None
    curr = head
    
    while curr:
        next_node = curr.next  # Store the next node
        curr.next = prev       # Reverse the current node's pointer
        prev = curr            # Move prev to current node
        curr = next_node       # Move to the next node
    
    return prev  # prev will be the new head after the loop
```
### Approach 2: Recursive Solution
##### Intuition: 
The recursive approach works by breaking the problem into smaller subproblems. The idea is to reverse the rest of the list first and then make the current node point to its previous node. This approach implicitly uses the call stack to keep track of the nodes as we recursively process them.

Steps:
1. If the head is None or head.next is None, return head (base case: the list is empty or has one node).
2. Recursively reverse the rest of the list: rest = reverseList(head.next).
3. After reversing the rest, set head.next.next = head to make the next node point back to the current node.
4. Set head.next = None to avoid a cycle in the reversed list.
5. Return the rest, which is the new head of the reversed list.

##### Visualization:
For head = [1, 2, 3, 4, 5]:

```perl
Recursive call: reverseList([2, 3, 4, 5])
Recursive call: reverseList([3, 4, 5])
...

Base case:
reverseList([5]) → return [5]

Backtrack:
4.next.next = 4 → list becomes [5, 4]
3.next.next = 3 → list becomes [5, 4, 3]
...
```
##### Time Complexity:
O(n), where n is the number of nodes in the list. We traverse the list once.
##### Space Complexity:
O(n), because of the recursive call stack that grows to a depth of n.
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseList(head: ListNode) -> ListNode:
    # Base case: if the list is empty or has only one node, return head
    if not head or not head.next:
        return head
    
    # Recursively reverse the rest of the list
    rest = reverseList(head.next)
    
    # Make the next node point back to the current node
    head.next.next = head
    # Set the current node's next to None (it becomes the new tail)
    head.next = None
    
    # Return the new head (the result of reversing the rest)
    return rest
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Iterative                    | O(n)      | O(1)             |
| Recursive                          | O(n)            | O(n)             |

Both the iterative and recursive solutions have the same time complexity, but the iterative solution is more space-efficient since it doesn't use the call stack.