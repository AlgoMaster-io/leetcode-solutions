# [Leetcode 24: Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

## Approaches

1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach](#iterative-approach)

---

### Recursive Approach

In a recursive approach, the idea is to think about how to swap the first two nodes, then recursively swap the rest of the list. 

- **Intuition**:
    - If there are less than two nodes left, return the head as there is nothing to swap.
    - Swap the first two nodes.
    - Make a recursive call for the rest of the list and attach the returned head to the current second node.
    - Return the head of the new list.

```javascript
function swapPairs(head) {
    // Base case: if there's no node or only one node left, return head
    if (!head || !head.next) {
        return head;
    }

    // Pointers to the first and second nodes
    let firstNode = head;
    let secondNode = head.next;

    // Swap: The head now should be the second node
    firstNode.next = swapPairs(secondNode.next);
    secondNode.next = firstNode;

    // Return the new head, which is the second node after swapping
    return secondNode;
}
```

- **Time Complexity**: O(n) because each node is visited once.
- **Space Complexity**: O(n) for recursion stack.

---

### Iterative Approach

The iterative approach helps in managing the swaps without recursion, which is helpful in environments where recursion depth is a concern.

- **Intuition**:
    - Use a dummy node to handle edge cases easily.
    - Traverse the list while swapping every two nodes.
    - Maintain pointers to keep track of the current two nodes to swap, and the node before them.
    - Re-adjust the pointers accordingly after each swap.

```javascript
function swapPairs(head) {
    // Dummy node to handle edge cases in the beginning
    let dummy = new ListNode(0);
    dummy.next = head;
    
    let prevNode = dummy;
    
    while (head && head.next) {
        // Nodes to be swapped
        let firstNode = head;
        let secondNode = head.next;
        
        // Swapping
        prevNode.next = secondNode;
        firstNode.next = secondNode.next;
        secondNode.next = firstNode;
        
        // Reinitializing the head and prevNode for next swap
        prevNode = firstNode;
        head = firstNode.next; // Move head to the next pair
    }
    
    // The new head of the list
    return dummy.next;
}
```

- **Time Complexity**: O(n) because we swap every pair and traverse through the list once.
- **Space Complexity**: O(1) as it uses constant extra space.

---

Both approaches effectively swap every two adjacent nodes in a linked list, but the iterative approach is often preferred for saving stack space when the recursion depth could become significant.

