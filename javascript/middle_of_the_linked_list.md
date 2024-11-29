# [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

## Approach 1: Two Passes (Count Nodes)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    middleNode(head) {
        let count = 0;
        let current = head;

        // Count the total number of nodes
        while (current !== null) {
            count++;
            current = current.next;
        }

        // Find the middle index
        let middleIndex = Math.floor(count / 2);
        current = head;

        // Move to the middle node
        for (let i = 0; i < middleIndex; i++) {
            current = current.next;
        }

        return current;
    }
}
```

## Approach 2: Fast and Slow Pointers (Optimal)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    middleNode(head) {
        let slow = head;
        let fast = head;

        // Move fast pointer twice as fast as the slow pointer
        while (fast !== null && fast.next !== null) {
            slow = slow.next; // Move slow pointer one step
            fast = fast.next.next; // Move fast pointer two steps
        }

        return slow; // Slow pointer will be at the middle
    }
}
```

