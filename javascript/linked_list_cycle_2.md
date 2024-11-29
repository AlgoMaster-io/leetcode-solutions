# [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approach 1: Using Set to Track Visited Nodes

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)

class ListNode {
    constructor(val) {
        this.val = val;
        this.next = null;
    }
}

var detectCycle = function(head) {
    const visited = new Set();

    while (head !== null) {
        // If the node has been visited before, it is the start of the cycle
        if (visited.has(head)) {
            return head;
        }
        visited.add(head);
        head = head.next;
    }

    return null; // No cycle detected
};
```

## Approach 2: Floyd's Cycle Detection Algorithm (Optimal)

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)

class ListNode {
    constructor(val) {
        this.val = val;
        this.next = null;
    }
}

var detectCycle = function(head) {
    if (head === null || head.next === null) {
        return null; // No cycle possible
    }

    let slow = head;
    let fast = head;

    // Detect if a cycle exists
    while (fast !== null && fast.next !== null) {
        slow = slow.next;
        fast = fast.next.next;

        if (slow === fast) { // Cycle detected
            let pointer = head;

            // Find the start of the cycle
            while (pointer !== slow) {
                pointer = pointer.next;
                slow = slow.next;
            }

            return pointer; // Start of the cycle
        }
    }

    return null; // No cycle detected
};
```

