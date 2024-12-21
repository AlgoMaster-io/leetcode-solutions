# [LeetCode 141: Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Table of Contents
- [Approach 1: Using a Set](#approach-1-using-a-set)
- [Approach 2: Floyd's Tortoise and Hare (Cycle Detection)](#approach-2-floyds-tortoise-and-hare-cycle-detection)

## Approach 1: Using a Set

### Intuition
The simplest way to determine if a linked list has a cycle is by keeping track of visited nodes. We can use a Set to store nodes that we have visited as we traverse the list. If we encounter a node that is already in the set, we have found a cycle. The node at which we first detect a repeat is the start of the cycle.

### Steps
1. Initialize an empty set to keep track of visited nodes.
2. Traverse the linked list.
   - For each node, check if it is in the set.
   - If it is, return that node as it indicates the entry point of the cycle.
   - If it's not in the set, add it to the set.
3. If we reach the end of the list (null), return null as there is no cycle.

### JavaScript Code
```javascript
function detectCycle(head) {
    // Initialize a set to keep track of visited nodes
    const visitedNodes = new Set();
    
    let current = head;
    // Traverse the linked list
    while (current !== null) {
        // Check if the current node is already visited
        if (visitedNodes.has(current)) {
            // Cycle detected, return the entry point of the cycle
            return current;
        }
        // Add the current node to the visited set
        visitedNodes.add(current);
        // Move to the next node
        current = current.next;
    }
    // No cycle detected, return null
    return null;
}
```

### Time Complexity
- O(N), where N is the total number of nodes in the linked list. We potentially visit each node once.

### Space Complexity
- O(N), because we store each node reference in the set.

---

## Approach 2: Floyd's Tortoise and Hare (Cycle Detection)

### Intuition
A more space-efficient approach relies on Floyd's Tortoise and Hare algorithm. This method uses two pointers, `slow` and `fast`. `slow` moves one step at a time, while `fast` moves two steps. If there's a cycle, these two pointers will meet within the cycle. After detecting the cycle, we can find the entry point of the cycle by restarting one pointer from the head and moving both pointers one step at a time.

### Steps
1. Initialize two pointers, `slow` and `fast`, to the head of the list.
2. Move `slow` by one step and `fast` by two steps until `fast` equals `null` or they meet.
   - If they meet, it indicates a cycle in the list.
3. Reset one pointer to the head, and keep the other at the meeting point.
4. Move both one step at a time; the point where they meet again is the start of the cycle.

### JavaScript Code
```javascript
function detectCycle(head) {
    // Initialize slow and fast pointers
    let slow = head;
    let fast = head;
    
    // Move slow by 1 step and fast by 2 steps
    while (fast !== null && fast.next !== null) {
        slow = slow.next; // Move slow by 1
        fast = fast.next.next; // Move fast by 2
        
        // If they meet, cycle is present
        if (slow === fast) {
            let start = head;
            while (start !== slow) {
                // Move both pointers one step at a time
                start = start.next;
                slow = slow.next;
            }
            // Return the start of the cycle
            return start;
        }
    }
    // No cycle detected
    return null;
}
```

### Time Complexity
- O(N), where N is the total number of nodes in the list. Each pointer traverses the list.

### Space Complexity
- O(1), as we only use two pointer variables and a few other fixed-space variables.

These two methods provide different insights into cycle detection with trade-offs in terms of space and simplicity. Floyd's Tortoise and Hare algorithm is typically more efficient due to its constant space complexity.

