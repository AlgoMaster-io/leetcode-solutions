# [Leetcode 206: Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

## Approaches

- [Iterative Approach](#iterative-approach)
- [Recursive Approach](#recursive-approach)

### Iterative Approach

#### Intuition
The iterative approach to reversing a linked list is straightforward. The idea is to traverse the list and invert the `next` pointer of each node to point to the previous node. We maintain three pointers: `prev`, `current`, and `next`. As we iterate over the linked list, we adjust pointers to reverse the link directions.

#### Steps
1. Initialize `prev` to `null` and `current` to the head of the list.
2. Iterate over the list:
   - Keep track of the `current.next` node with a temporary pointer `next`.
   - Reverse the link by setting `current.next` to `prev`.
   - Move `prev` and `current` one step forward.
3. At the end, `prev` will point to the new head of the reversed list.

#### Time and Space Complexity
- **Time Complexity:** O(n), where n is the number of nodes in the list.
- **Space Complexity:** O(1), as we are using only a constant amount of extra space.

```javascript
function reverseList(head) {
    let prev = null;
    let current = head;

    while (current !== null) {
        // Save the current next node
        let next = current.next;
        // Reverse the link
        current.next = prev;
        // Move prev and current one step forward
        prev = current;
        current = next;
    }
    
    // prev will be the new head of the reversed list
    return prev;
}
```

### Recursive Approach

#### Intuition
The recursive approach takes advantage of the call stack to reverse the linked list. The idea is to recursively reverse the rest of the list and then set the next node of the current node to point back to the current node itself.

#### Steps
1. Base Case: If the head is `null` or there is only one node, return head.
2. Recurse for the rest of the list and get the reversed list's new head.
3. Make the next node of `head` point back to `head`.
4. Set `head.next` to `null` to break the old link.
5. Return the new head.

#### Time and Space Complexity
- **Time Complexity:** O(n), where n is the number of nodes in the list.
- **Space Complexity:** O(n), due to the recursion stack.

```javascript
function reverseList(head) {
    // Base case: if head is null or only one node, return it
    if (head === null || head.next === null) {
        return head;
    }

    // Recursively reverse the rest of the list
    let newHead = reverseList(head.next);

    // Make the next node of head point back to head
    head.next.next = head;
    // Set head.next to null
    head.next = null;

    // Return new head of the reversed list
    return newHead;
}
```

Navigating through these solutions gives an excellent understanding of both iterative and recursive approaches to reverse a linked list, along with their trade-offs in terms of space and time complexity.

