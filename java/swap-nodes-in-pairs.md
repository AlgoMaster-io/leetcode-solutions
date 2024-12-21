# [Leetcode 24: Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

## Approaches

1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach](#iterative-approach)

### Recursive Approach

The recursive approach involves swapping nodes in pairs and continues until the end of the linked list is reached. Here's the idea:

1. **Base Case**: If the list is empty or has only one node, no swap is needed. Return the head.
2. **Recursive Step**:
   - Swap the first two nodes.
   - Continue swapping for the rest of the list using recursion.

#### Code
```java
public ListNode swapPairsRecursive(ListNode head) {
    // Base case: If list is empty or has only one node, no swap is needed
    if (head == null || head.next == null) {
        return head;
    }

    // Nodes to be swapped
    ListNode firstNode = head;
    ListNode secondNode = head.next;

    // Swapping process - secondNode becomes the new head
    firstNode.next = swapPairsRecursive(secondNode.next);
    secondNode.next = firstNode;

    // Return the new head of the swapped pair
    return secondNode;
}
```
#### Time Complexity
- Each pair of nodes is processed in constant time. Hence, the time complexity is O(N), where N is the number of nodes in the linked list.

#### Space Complexity
- O(N) due to recursion stack space used for each node.

---

### Iterative Approach

The iterative approach involves using a dummy node to facilitate the swapping process as we walk through the list:

1. **Setup a Dummy Node**: This helps manage edge cases and makes node swapping easier.
2. **Traversal and Swapping**:
   - Swap nodes by adjusting pointers.
   - Use a loop to continue swapping until the end of the list is reached.

#### Code
```java
public ListNode swapPairsIterative(ListNode head) {
    // Initial dummy node to simplify edge cases
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode current = dummy;

    // Traverse and swap pairs while there are at least two nodes remaining
    while (current.next != null && current.next.next != null) {
        // Nodes to be swapped
        ListNode firstNode = current.next;
        ListNode secondNode = current.next.next;

        // Adjusting pointers for swap
        firstNode.next = secondNode.next;
        current.next = secondNode;
        secondNode.next = firstNode;

        // Move the pointer by two nodes for next pair
        current = firstNode;
    }

    // Return new head at dummy's next
    return dummy.next;
}
```
#### Time Complexity
- O(N) since we traverse each node of the list exactly once.

#### Space Complexity
- O(1) as no additional space is used other than a few pointers.

Both approaches yield the correct result, with the recursive approach being elegant but less optimal space-wise, and the iterative approach being efficient in both time and space execution.

