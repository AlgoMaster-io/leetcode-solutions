# [Leetcode 24: Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

## Table of Contents
- [Approach 1: Iterative Approach](#approach-1)
- [Approach 2: Recursive Approach](#approach-2)

## Approach 1: Iterative Approach

### Intuition:
The aim is to swap every pair of adjacent nodes in the linked list. An iterative approach is straightforward: we traverse the list, and for every pair of nodes, we change their links to swap them.

### Steps:
1. Create a dummy node that points to the head of the list. This helps to easily return the new head.
2. Use a pointer `prev` to manage the previous node before the pair being swapped and `current` for the current node.
3. Iterate through the list while there are at least two more nodes ahead.
4. For each pair of nodes:
   - Identify the two nodes of the pair (`first` and `second`).
   - Adjust the pointers to swap them.
   - Advance the `prev` and `current` pointers to the next pair's beginning.
5. Return the new head of the list.

### Code:
```csharp
public ListNode SwapPairs(ListNode head) {
    if (head == null || head.next == null) return head;

    // Dummy node to simplify the handling of head swaps
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode prev = dummy;

    // Traverse the list
    while (prev.next != null && prev.next.next != null) {
        ListNode first = prev.next;
        ListNode second = prev.next.next;

        // Swapping nodes
        first.next = second.next;
        second.next = first;
        prev.next = second;

        // Move prev to the new previous of the next pair
        prev = first;
    }

    return dummy.next;
}
```

### Complexity:
- **Time Complexity:** O(n) - We pass through the list only once, where n is the number of nodes.
- **Space Complexity:** O(1) - We use a constant amount of extra space without using any additional data structures.

## Approach 2: Recursive Approach

### Intuition:
Recursion provides a very neat solution by breaking down the problem into smaller sub-problems. Each recursive call deals with two nodes at a time, swapping them and recursively processing the rest of the list.

### Steps:
1. Base case: If `head` is null or there's only one node left, return `head`.
2. Initialize two pointers, `first` for the current node and `second` for the next node.
3. Recursively call the function for the list starting after the second node.
4. Swap `first` and `second`.
5. Return `second` as the new head.

### Code:
```csharp
public ListNode SwapPairs(ListNode head) {
    // Base case: If there's no node or only one node, there's no swap needed
    if (head == null || head.next == null) return head;

    // Nodes to be swapped
    ListNode first = head;
    ListNode second = head.next;

    // Recursively swapping next pairs
    first.next = SwapPairs(second.next);
    second.next = first;

    // Return the new head of the swapped pair
    return second;
}
```

### Complexity:
- **Time Complexity:** O(n) - Every node is visited once, where n is the number of nodes.
- **Space Complexity:** O(n) - Due to recursive stack calls using space proportional to the depth of recursion.

