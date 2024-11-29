# [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

## Approach 1: Iterative Solution

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function reverseList(head) {
    let prev = null; // Previous node, initially null
    let current = head; // Current node to process

    while (current !== null) {
        let nextTemp = current.next; // Store the next node
        current.next = prev; // Reverse the link
        prev = current; // Move prev to the current node
        current = nextTemp; // Move to the next node
    }

    return prev; // New head of the reversed list
}
```

## Approach 2: Recursive Solution

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n) (due to recursion stack)
function reverseList(head) {
    // Base case: if the list is empty or has only one node
    if (head === null || head.next === null) {
        return head;
    }

    // Reverse the rest of the list
    const reversedList = reverseList(head.next);

    // Reverse the current node
    head.next.next = head;
    head.next = null;

    return reversedList; // Return the new head
}
```

