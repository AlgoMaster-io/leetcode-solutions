# Remove Nth Node From End of List
Given the head of a linked list, remove the n-th node from the end of the list and return its head.

### Constraints:
- The number of nodes in the list is sz (1 ≤ sz ≤ 30).
- 0 <= Node.val <= 100
- 1 <= n <= sz

### Examples
```javascript
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
Explanation: The 2nd node from the end is 4, so it gets removed.

Input: head = [1], n = 1
Output: []
Explanation: The only node in the list is removed.

Input: head = [1,2], n = 1
Output: [1]
```

## Approaches to Solve the Problem
### Approach 1: Two-Pass Solution (Finding Length First)
##### Intuition:
The simplest approach is to first calculate the length of the linked list and then remove the (length - n)-th node from the beginning.

1. Traverse the list to find the total length.
2. Calculate the position of the node from the start: length - n.
3. Traverse the list again to this position and remove the node.

Steps:
1. Traverse the list to find its length.
2. Traverse the list again, and stop at the node just before the (length - n)-th node.
3. Adjust the next pointer of the previous node to remove the n-th node.
##### Time Complexity:
O(L), where L is the length of the linked list. We traverse the list twice.
##### Space Complexity:
O(1), as we only use a few extra variables.
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def removeNthFromEnd(head: ListNode, n: int) -> ListNode:
    # Step 1: Find the total length of the list
    length = 0
    current = head
    while current:
        length += 1
        current = current.next
    
    # Step 2: Remove the (length - n)-th node
    dummy = ListNode(0)
    dummy.next = head
    current = dummy
    
    for _ in range(length - n):
        current = current.next
    
    # Skip the target node
    current.next = current.next.next
    
    return dummy.next
```
### Approach 2: One-Pass Solution with Two Pointers (Optimal Solution)
##### Intuition: 
To solve the problem in one pass, we can use two pointers. The idea is to maintain a gap of n nodes between two pointers: fast and slow. By the time fast reaches the end of the list, slow will be just before the node that needs to be removed.

Steps:
1. Initialize two pointers fast and slow to the dummy node.
2. Move fast ahead by n nodes.
3. Move both fast and slow one node at a time until fast reaches the end.
4. Now, slow is at the node just before the node to be removed. Adjust the next pointer to skip the target node.

##### Visualization:
```perl
For head = [1,2,3,4,5] and n = 2:

Initially:
fast -> 1 -> 2 -> 3 -> 4 -> 5
slow -> 1 -> 2 -> 3 -> 4 -> 5

Move fast ahead by 2 steps:
fast -> 3 -> 4 -> 5
slow -> 1 -> 2 -> 3 -> 4 -> 5

Move both pointers:
fast -> 5
slow -> 3 -> 4 -> 5

Now, slow points to the node just before the node to remove (4). We skip 4.
```
##### Time Complexity:
O(L), where L is the length of the linked list. We traverse the list only once.
##### Space Complexity:
O(1), as we only use constant extra space for the two pointers.
##### Python Code:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def removeNthFromEnd(head: ListNode, n: int) -> ListNode:
    dummy = ListNode(0)
    dummy.next = head
    fast = dummy
    slow = dummy
    
    # Move fast n steps ahead
    for _ in range(n):
        fast = fast.next
    
    # Move both fast and slow until fast reaches the end
    while fast.next:
        fast = fast.next
        slow = slow.next
    
    # Skip the target node
    slow.next = slow.next.next
    
    return dummy.next
```
#### Edge Cases:
1. Removing the Head Node:
If n is equal to the length of the list, the node to be removed is the head. In such cases, we must handle the head carefully and return head.next.

2. Single Node List:
If the list contains only one node and n = 1, removing the only node results in an empty list.

3. General Case:
When n is in the middle or near the end of the list, the solution should work seamlessly.
## Summary

| Approach                         | Time Complexity | Space Complexity |
|-----------------------------------|-----------------|------------------|
| Two-Pass Solution (Length First)	                    | O(L)      | O(1)             |
| One-Pass Solution (Two Pointers)	                          | O(L)            | O(1)             |

The One-Pass Solution (Two Pointers) is the most optimal approach since it achieves the result in a single traversal of the linked list with constant space usage.