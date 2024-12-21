# [Leetcode 206: Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

## Table of Contents
1. [Iterative Approach](#iterative-approach)
2. [Recursive Approach](#recursive-approach)

## Iterative Approach

The iterative approach uses a loop to traverse through the list and change the `next` pointers of each node so that it points to the previous node, effectively reversing the list.

### Intuition

In the iterative approach, we maintain three pointers:
1. `prev`: Initially set to `null`. It will end up being the new head of the reversed list.
2. `current`: This starts at the head of the list and moves forward with each iteration.
3. `next`: This temporarily stores the `next` node from `current` to help in the transition.

The idea is to iterate over each node and reverse its `next` pointer to point to the previous node (`prev`), then move `prev` and `current` one step forwards.

### Implementation

```csharp
public ListNode ReverseList(ListNode head) {
    ListNode prev = null;
    ListNode current = head;
    
    while (current != null) {
        // Store the next node before breaking the link
        ListNode next = current.next;
        
        // Reverse the link
        current.next = prev;
        
        // Move prev and current one step forward
        prev = current;
        current = next;
    }
    
    // Prev will be the new head of the reversed list
    return prev;
}

```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the linked list. We traverse each node once.
- **Space Complexity**: O(1), we use a constant amount of extra space.

## Recursive Approach

In the recursive approach, we reverse the rest of the linked list and then append the current node to the end of that reversed list. The base case is when we reach the end of the list.

### Intuition

Here's the recursive logic step-by-step:
1. Recursively reverse the sublist from the second node onwards.
2. Set the `next` of the node just returned by the recursion to point back to the current node.
3. Clear the `next` of the current node to prevent cycles.
4. Return the reversed sublist head.

### Implementation

```csharp
public ListNode ReverseList(ListNode head) {
    // Base case: If the list is empty or has only one node
    if (head == null || head.next == null) {
        return head;
    }
    
    // Recursive step: reverse the list from the second node onward
    ListNode newHead = ReverseList(head.next);
    
    // Reverse the current node's pointer
    head.next.next = head;
    head.next = null;
    
    // Return the new head of the reversed list
    return newHead;
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of nodes in the linked list. Each node is visited once.
- **Space Complexity**: O(n), due to the recursion stack. In the worst case, the stack size grows linearly with the input size (n), called the recursion depth.

