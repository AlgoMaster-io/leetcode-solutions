
# Middle of the Linked List
You are given the head of a singly linked list. Return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

### Constraints:
The number of nodes in the list is between 1 and 100.
1 <= Node.val <= 100


### Examples
```javascript
Input: head = [1, 2, 3, 4, 5]
Output: [3, 4, 5]
Explanation: The middle node of the list is 3.

Input: head = [1, 2, 3, 4, 5, 6]
Output: [4, 5, 6]
Explanation: Since the list has two middle nodes with values 3 and 4, the second middle node is returned.
```

## Approaches to Solve the Problem
### Approach 1: Traverse and Count Nodes (Two Passes)
##### Intuition:
The simplest way to find the middle node is to first count the total number of nodes and then traverse again to the middle. We do this in two passes: one to count the nodes and another to reach the middle.

Traverse the list and count the total number of nodes, n.
In the second pass, traverse up to n // 2 to get the middle node.
##### Time Complexity:
O(n), where n is the number of nodes in the linked list.
##### Space Complexity:
O(1), no extra space is used beyond a few variables.
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def middleNode(head: ListNode) -> ListNode:
    # First pass: count the nodes
    count = 0
    current = head
    while current:
        count += 1
        current = current.next

    # Second pass: go to the middle node
    mid = count // 2
    current = head
    for i in range(mid):
        current = current.next

    return current
```
### Approach 2: Single Pass with Two Pointers (Optimal Solution)
##### Intuition: 
A more efficient approach is to use two pointers: slow and fast. The slow pointer moves one step at a time, while the fast pointer moves two steps. When the fast pointer reaches the end, the slow pointer will be at the middle of the list.

1. Initialize both slow and fast pointers to the head of the list.
2. Move slow one step at a time and fast two steps at a time.
3. When fast reaches the end (None), slow will be at the middle.

##### Why This Works:
Since fast is moving twice as fast, when fast reaches the end, slow will have traversed half the list.
##### Time Complexity:
O(n), because we traverse the list only once.
##### Space Complexity:
O(1), no extra space beyond the two pointers is used.
##### Visualization:
```rust
Linked List: 1 -> 2 -> 3 -> 4 -> 5

slow moves 1 step, fast moves 2 steps:
    slow -> 1
    fast -> 2

Next step:
    slow -> 2
    fast -> 4

Next step:
    slow -> 3
    fast -> None (end)

Result: slow points to 3, which is the middle node.

```
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def middleNode(head: ListNode) -> ListNode:
    slow = head
    fast = head
    
    # Move slow one step and fast two steps
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    # When fast reaches the end, slow is at the middle
    return slow
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Traverse and Count (Two Passes)   | O(n)            | O(1)             |
| Two Pointers (Single Pass)        | O(n)            | O(1)             |

The two pointers approach is the most optimal solution because it only requires one pass and uses constant space. This approach is widely used for finding the middle of linked lists or solving problems that involve finding specific nodes in linked lists with minimal space and time overhead.