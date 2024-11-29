# [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approach 1: Using HashSet to Track Visited Nodes

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def detectCycle(self, head):
        visited = set()
        
        while head:
            # If the node has been visited before, it is the start of the cycle
            if head in visited:
                return head
            visited.add(head)
            head = head.next
            
        return None  # No cycle detected
```

## Approach 2: Floyd's Cycle Detection Algorithm (Optimal)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def detectCycle(self, head):
        if head is None or head.next is None:
            return None  # No cycle possible

        slow = head
        fast = head

        # Detect if a cycle exists
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:  # Cycle detected
                pointer = head

                # Find the start of the cycle
                while pointer != slow:
                    pointer = pointer.next
                    slow = slow.next
                
                return pointer  # Start of the cycle
                
        return None  # No cycle detected
```

