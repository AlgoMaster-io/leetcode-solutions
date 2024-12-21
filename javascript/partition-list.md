# [Leetcode 86: Partition List](https://leetcode.com/problems/partition-list/)

## Solutions
- [Approach 1: Two Pointers Technique](#approach-1-two-pointers-technique)

### Approach 1: Two Pointers Technique

**Intuition**:  
The key idea is to rearrange the nodes of the given linked list using two separate lists: one for nodes less than the given value `x` and another for nodes greater or equal to `x`. These lists are then combined to form the desired partitioned list. We maintain two pointers, `before` and `after`, to build these two separate lists as we iterate through the linked list.

This strategy allows us to traverse the list once, filtering nodes into two partitions and then combining them in the desired order.

**Steps**:
1. Create two new dummy nodes, `before_head` and `after_head`, to represent the start of the two partitions.
2. Use two pointers, `before` and `after`, that will build the two partitions using the dummy nodes.
3. Traverse the given list, inspecting each node:
   - If the node's value is less than `x`, append it to the `before` list.
   - Otherwise, append it to the `after` list.
4. Once all nodes have been assigned to a partition, connect the end of the `before` list to the start of the `after` list. Ensure the `after` list ends with a null pointer to terminate the combined list properly.
5. The result is the list starting from `before_head.next`.

```javascript
var partition = function(head, x) {
    // Dummy nodes to help in building result linked lists
    let before_head = new ListNode(0);
    let after_head = new ListNode(0);
    
    // Use two pointers to traverse and build the linked list
    let before = before_head;
    let after = after_head;
    
    // Traverse the original linked list
    while (head !== null) {
        if (head.val < x) {
            // Append node to the end of the 'before' list
            before.next = head;
            before = before.next;
        } else {
            // Append node to the end of the 'after' list
            after.next = head;
            after = after.next;
        }
        // Move to the next node in the original list
        head = head.next;
    }
    
    // Connect the 'before' list with 'after' list
    before.next = after_head.next;
    // Ensure the 'after' end is null
    after.next = null;
    
    // Return the start of the 'before' list, skipping dummy node
    return before_head.next;
};
```

**Time Complexity**: O(n), where n is the number of nodes in the linked list. We iterate through the list only once.
  
**Space Complexity**: O(1), as we are reusing the nodes from the original list rather than creating new nodes, aside from the dummy nodes used temporarily.

