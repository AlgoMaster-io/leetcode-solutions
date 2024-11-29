# [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approach 1: Using HashSet to Track Visited Nodes

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)

class ListNode {
    val: number;
    next: ListNode | null;
  
    constructor(val?: number, next?: ListNode | null) {
        this.val = val || 0;
        this.next = next || null;
    }
}

function detectCycle(head: ListNode | null): ListNode | null {
    const visited = new Set<ListNode>();

    while (head !== null) {
        // If the node has been visited before, it is the start of the cycle
        if (visited.has(head)) {
            return head;
        }
        visited.add(head);
        head = head.next;
    }

    return null; // No cycle detected
}
```

## Approach 2: Floyd's Cycle Detection Algorithm (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)

class ListNode {
    val: number;
    next: ListNode | null;
  
    constructor(val?: number, next?: ListNode | null) {
        this.val = val || 0;
        this.next = next || null;
    }
}

function detectCycle(head: ListNode | null): ListNode | null {
    if (head === null || head.next === null) {
        return null; // No cycle possible
    }

    let slow: ListNode | null = head;
    let fast: ListNode | null = head;

    // Detect if a cycle exists
    while (fast !== null && fast.next !== null) {
        slow = slow.next!;
        fast = fast.next.next;

        if (slow === fast) { // Cycle detected
            let pointer: ListNode | null = head;

            // Find the start of the cycle
            while (pointer !== slow) {
                pointer = pointer.next!;
                slow = slow.next!;
            }

            return pointer; // Start of the cycle
        }
    }

    return null; // No cycle detected
}
```

