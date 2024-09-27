
# Linked List Cycle
Given the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, the position is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

### Constraints:
The number of nodes in the list is in the range [0, 10^4].
-10^5 <= Node.val <= 10^5
pos is -1 or a valid index in the linked list.

### Examples
```javascript
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.

Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

## Approaches to Solve the Problem
### Approach 1: Hash Set (Using Extra Space)
##### Intuition:
The most intuitive way to solve the problem is by tracking each node we visit. We can store all visited nodes in a hash set. If we encounter a node that has already been visited, it means there's a cycle.

1. Traverse the linked list.
2. Add each node to a hash set.
3. If a node is already present in the set, return True (cycle exists).
4. If the traversal completes without finding a repeated node, return False (no cycle).
##### Time Complexity:
O(n), where n is the number of nodes in the linked list. We traverse the list once.
##### Space Complexity:
O(n), because we store each node in the hash set.
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def hasCycle(head: ListNode) -> bool:
    visited = set()  # Hash set to store visited nodes
    current = head
    
    while current:
        if current in visited:
            return True  # Cycle detected
        visited.add(current)
        current = current.next
    
    return False  # No cycle
```
### Approach 2: Two Pointers (Floyd’s Cycle Detection Algorithm)
##### Intuition: 
This approach, also known as the Tortoise and Hare algorithm, uses two pointers (slow and fast). The slow pointer moves one step at a time, while the fast pointer moves two steps. If there's a cycle, the two pointers will eventually meet. If there's no cycle, the fast pointer will reach the end of the list.

1. Initialize both slow and fast pointers to the head.
2. Move slow one step and fast two steps.
3. If slow and fast meet, there is a cycle.
4. If fast reaches the end (None), there is no cycle.

##### Why This Works:
If a cycle exists, the fast pointer, moving twice as fast as the slow pointer, will "lap" the slow pointer and meet it again inside the cycle.
##### Time Complexity:
O(n), because in the worst case, we traverse the list only once.
##### Space Complexity:
O(1), since no extra space is used except for the two pointers.
##### Visualization:
```rust
List with a cycle: 1 -> 2 -> 3 -> 4 -> 5 -> 2 (cycle starts again)

slow moves 1 step, fast moves 2 steps:
    slow -> 2
    fast -> 3

Next step:
    slow -> 3
    fast -> 5

Next step:
    slow -> 4
    fast -> 3 (caught up inside the cycle)

Both pointers meet, cycle detected.
```
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def hasCycle(head: ListNode) -> bool:
    if not head or not head.next:
        return False  # No cycle if the list is empty or has only one node

    slow = head
    fast = head
    
    # Move slow by 1 step and fast by 2 steps
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        # If they meet, there's a cycle
        if slow == fast:
            return True
    
    # If fast reaches the end, there's no cycle
    return False
```
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Hash Set (Extra Space)   | O(n)            | O(n)             |
| Two Pointers (Floyd’s Algorithm)        | O(n)            | O(1)             |

The Two Pointers (Floyd’s Algorithm) is the most optimal solution as it uses constant space and detects the cycle in a single pass.