# [Leetcode 19: Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approaches
- [Approach 1: Brute Force with List](#approach-1)
- [Approach 2: Two-Pass with Length Calculation](#approach-2)
- [Approach 3: One-Pass with Two Pointers](#approach-3)

## Approach 1: Brute Force with List
### Intuition
The simplest approach is to first traverse the list and store each node in an indexed data structure, such as a list. This allows us to easily calculate the index of the node that needs to be removed when counted from the end.

1. Traverse the linked list and add each node to a list.
2. Calculate the position of the node to be removed using the list’s length.
3. Handle edge cases separately, such as when the head needs to be removed.
4. Remove the target node by adjusting pointers.

### Time and Space Complexity
- **Time Complexity**: `O(n)` because we traverse the list twice.
- **Space Complexity**: `O(n)` due to the list used to store the nodes.

```csharp
public ListNode RemoveNthFromEnd(ListNode head, int n) {
    List<ListNode> nodeList = new List<ListNode>();
    ListNode current = head;
    
    // Store all nodes in a list
    while (current != null) {
        nodeList.Add(current);
        current = current.next;
    }
    
    int targetIndex = nodeList.Count - n;
    
    // If we're removing the head node
    if (targetIndex == 0) {
        return head.next;
    }
    
    // Remove target node by linking previous node to the next node
    nodeList[targetIndex - 1].next = nodeList[targetIndex].next;
    
    return head;
}
```

## Approach 2: Two-Pass with Length Calculation
### Intuition
This approach calculates the length of the list in the first pass. With this length, we determine the position of the node to remove from the beginning.

1. Traverse to find the total length.
2. Compute the position from the start of the node to be deleted.
3. Traverse again to the node positioned before the one to be deleted.
4. Adjust pointers to remove it.

### Time and Space Complexity
- **Time Complexity**: `O(n)` because of two full traversals.
- **Space Complexity**: `O(1)` as no additional data structures are used.

```csharp
public ListNode RemoveNthFromEnd(ListNode head, int n) {
    int length = 0;
    ListNode current = head;
    
    // Calculate the length of the list
    while (current != null) {
        length++;
        current = current.next;
    }
    
    // Calculate the position from start
    int targetIndex = length - n;
    
    // If the first node needs to be removed
    if (targetIndex == 0) {
        return head.next;
    }

    current = head;
    // Traverse to the node just before the one to remove
    for (int i = 1; i < targetIndex; i++) {
        current = current.next;
    }
    
    // Skip the node to be removed
    current.next = current.next.next;
    
    return head;
}
```

## Approach 3: One-Pass with Two Pointers
### Intuition
This is the optimal solution using two pointers (fast and slow). Fast pointer advances n steps ahead first. Then both pointers move until the fast reaches the end. The slow pointer will then be at the node before the one to delete.

1. Move the fast pointer n steps ahead.
2. Move both fast and slow together until fast reaches the end.
3. Delete the node after the slow pointer.

### Time and Space Complexity
- **Time Complexity**: `O(n)` because we make a single pass through the list.
- **Space Complexity**: `O(1)` as it uses a constant amount of extra space.

```csharp
public ListNode RemoveNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode slow = dummy;
    ListNode fast = dummy;
    
    // Advance fast by n+1 steps
    for (int i = 0; i <= n; i++) {
        fast = fast.next;
    }
    
    // Move both pointers until fast reaches the end
    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }
    
    // Remove n-th node from end
    slow.next = slow.next.next;
    
    return dummy.next;
}
```

This optimal solution is efficient and elegant, ensuring a linear time complexity without the need for extra space.

