## [Problem: Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

### Approaches:
- [Approach 1: Recursive Method](#approach-1-recursive-method)
- [Approach 2: Iterative Method](#approach-2-iterative-method)

## Approach 1: Recursive Method

The basic idea is to reverse a group of `k` nodes using recursion. We reverse nodes one group at a time and pass the reversed linked list along for further processing. The main advantage of recursion here is its simplicity in handling the reversal:

### Intuition:
1. Count nodes to ensure the length of the remaining list is at least `k`. If not, return the head as no reversal is needed.
2. Reverse the first `k` nodes using recursion, and use the recursive call to reverse the remaining nodes.
3. Connect the originally first node (now last in the current reversed segment) to the next reversed segment.
4. Return the new head of the reversed list.

### Code:
```javascript
function reverseKGroup(head, k) {
    if (!head || k === 1) return head;

    // Function to check the length of the list starting from node
    const getLength = (node) => {
        let length = 0;
        while (node) {
            length++;
            node = node.next;
        }
        return length;
    };

    // Reverse function that reverses the linked list between start and end nodes
    const reverse = (start, end) => {
        let prev = null;
        let curr = start;
        while (curr !== end) {
            let next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev; // New head after reversal
    };

    let length = getLength(head);
    if (length < k) return head;

    // Initialize pointers
    let current = head;
    let prev = null;
    
    // Reverse at least k nodes
    for (let i = 0; i < k; i++) {
        let next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }

    // Recursively reverse the rest of the list beyond current
    head.next = reverseKGroup(current, k);

    return prev; // prev is the new head after the k-group reverse
}
```

### Complexity:
- **Time Complexity:** \(O(n)\) - We make one pass through the list, with each node being processed once.
- **Space Complexity:** \(O(n/k)\) - Due to the recursion stack.

## Approach 2: Iterative Method

This approach handles reversal iteratively, which allows us to avoid the recursion overhead and manage all nodes in place using pointers.

### Intuition:
1. Calculate the length of the linked list.
2. Reverse nodes iteratively in groups of `k`. This is done by iterating over the list while maintaining four pointers: `prev`, `current`, `next`, and `tail`.
3. After reversing a group, connect the current tail to the next reversed group.

### Code:
```javascript
function reverseKGroup(head, k) {
    if (k === 1 || !head) return head;

    // Calculate length of the list
    const getLength = (node) => {
        let length = 0;
        while (node) {
            length++;
            node = node.next;
        }
        return length;
    };
    
    const length = getLength(head);
    if (length < k) return head;

    // Function to reverse a section of the list
    const reverse = (start, end) => {
        let prev = null;
        let curr = start;
        while (curr !== end) {
            let next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    };

    const dummy = { next: head };  // Dummy node for easier manipulation
    let prevGroupEnd = dummy;
    let current = head;

    for (let i = 0; i < Math.floor(length / k); i++) {
        let groupStart = current;
        let groupEnd = current;
        // Moving the groupEnd to the end of the current group
        for (let j = 0; j < k; j++) {
            groupEnd = groupEnd.next;
        }

        // Reverse the current k-group
        prevGroupEnd.next = reverse(groupStart, groupEnd);

        // Connect this group back to the rest of the list
        groupStart.next = groupEnd;
        
        // Move to the next group
        prevGroupEnd = groupStart;
        current = groupEnd;
    }

    return dummy.next;
}
```

### Complexity:
- **Time Complexity:** \(O(n)\) - Each node is processed a constant number of times.
- **Space Complexity:** \(O(1)\) - In-place reversal with no extra space required beyond variables.

This solution efficiently reverses the linked list in groups of `k` while maintaining an \(O(1)\) space overhead, which makes it optimal in terms of both time and space complexity.

