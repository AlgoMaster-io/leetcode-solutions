# [LeetCode 25: Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approaches
- [Approach 1: Brute Force Recursion](#approach-1-brute-force-recursion)
- [Approach 2: Iterative Solution](#approach-2-iterative-solution)

### Approach 1: Brute Force Recursion

In this approach, we use a recursive strategy to reverse every `k` nodes in the linked list. Here's the detailed intuition:

1. **Base Condition**: 
   - If the number of nodes left in the linked list is less than `k`, simply return the head as we do not need to reverse these nodes.

2. **Recursive Reversal**:
   - Find the `k+1`th node. This node will serve as a reference for the remaining nodes yet to be processed.
   - Reverse the first `k` nodes of the current segment.
   - Attach the returned head of the reversed next segment to the last node of the reversed current segment.

3. **Recursive Call**:
   - The recursion happens on the node next to the `k` reversed nodes.

```csharp
public ListNode ReverseKGroup(ListNode head, int k) {
    ListNode current = head;
    int count = 0;
    
    // Find the k+1 node
    while (current != null && count < k) {
        current = current.next;
        count++;
    }
    
    // If we have k nodes, then proceed with reversal
    if (count == k) {
        // Reverse first k nodes
        current = ReverseKGroup(current, k);
        while (count-- > 0) {
            ListNode temp = head.next;
            head.next = current;
            current = head;
            head = temp;
        }
        head = current;
    }
    
    return head;
}
```

- **Time Complexity**: O(N), where N is the number of nodes in the linked list since each node is processed once.
- **Space Complexity**: O(N/k) due to recursion stack used by reverse operations.

### Approach 2: Iterative Solution

This approach focuses on using an iterative strategy to solve the problem, which eliminates the recursion stack limitation.

1. **Count Segments**:
   - First, calculate the total number of nodes in the list.

2. **Divide and Rule**:
   - Iterate over the list while there are enough nodes to form `k`-length groups.
   - For each group of `k`, reverse the nodes and connect the previous group to the current group.

3. **Reverse in Place**:
   - To reverse, use three-pointer technique (`prev`, `current`, `next`).

```csharp
public ListNode ReverseKGroupIterative(ListNode head, int k) {
    if (head == null || k == 1) return head;

    ListNode dummy = new ListNode(0);
    dummy.next = head;

    ListNode current = dummy, next = dummy, prev = dummy;
    int count = 0;
    
    // Count the total number of nodes
    while (current.next != null) {
        current = current.next;
        count++;
    }
    
    while (count >= k) {
        current = prev.next;
        next = current.next;
        for (int i = 1; i < k; ++i) {
            current.next = next.next;
            next.next = prev.next;
            prev.next = next;
            next = current.next;
        }
        prev = current;
        count -= k;
    }
    
    return dummy.next;
}
```

- **Time Complexity**: O(N), similar to the recursive approach as every node is visited once for reversal.
- **Space Complexity**: O(1), iterative approach utilizes constant space.

This iterative version ensures we are more efficient in both memory usage as it avoids recursion stack buildup and still achieves the goal of reversing the linked nodes in groups of `k`.

