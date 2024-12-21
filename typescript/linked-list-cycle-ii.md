# [Leetcode 141: Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approaches

- [Approach 1: Brute Force - Using a Set](#approach-1-brute-force---using-a-set)
- [Approach 2: Floyd's Tortoise and Hare Algorithm](#approach-2-floyds-tortoise-and-hare-algorithm)

## Approach 1: Brute Force - Using a Set

### Intuition:
The simplest way to determine if a linked list has a cycle is by using a Set (or HashSet/Map). As we iterate through the list, we can store each node we visit in the set. If we encounter a node that is already in the set, then we know there's a cycle, and that node is where the cycle begins.

### Algorithm:
1. Initialize an empty set to keep track of visited nodes.
2. Start from the head of the linked list.
3. For each node, check if it's already in the set:
   - If it is, return that node as it is the start of the cycle.
   - If not, add it to the set.
4. If the loop terminates without finding a cycle, return null (no cycle).

### Code:

```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.next = (next===undefined ? null : next);
    }
}

function detectCycle(head: ListNode | null): ListNode | null {
    const visitedNodes: Set<ListNode> = new Set();

    let currentNode = head;

    while (currentNode !== null) {
        // If currentNode is already visited, there is a cycle.
        if (visitedNodes.has(currentNode)) {
            return currentNode;
        }
        // Add currentNode to the visited set
        visitedNodes.add(currentNode);
        // Move to the next node
        currentNode = currentNode.next;
    }

    // If no cycle was detected
    return null;
}
```

### Time and Space Complexity:
- **Time Complexity:** O(n) - Each node is processed at most once.
- **Space Complexity:** O(n) - We store each node in the set.

## Approach 2: Floyd's Tortoise and Hare Algorithm

### Intuition:
This approach uses two pointers: a slow pointer (tortoise) and a fast pointer (hare). The fast pointer moves twice as fast as the slow pointer. If there's a cycle in the list, the fast pointer will eventually meet the slow pointer inside the cycle. At that point, to find the cycle’s entry point, we reset one pointer to the start of the list and move both pointers one step at a time; they will meet at the cycle's starting node.

### Algorithm:
1. Initialize two pointers, `slow` and `fast`. Both start at the head of the list.
2. Move `slow` one step and `fast` two steps through the list:
   - If `fast` or `fast.next` becomes null, there's no cycle. Return null.
   - If `slow` equals `fast`, there's a cycle, break the loop.
3. Reset `slow` to the head.
4. Move both `slow` and `fast` one step at a time until they meet:
   - The node where they meet is the starting node of the cycle.

### Code:

```typescript
function detectCycle(head: ListNode | null): ListNode | null {
    if (!head || !head.next) return null;

    let slow: ListNode | null = head;
    let fast: ListNode | null = head;

    // Phase 1: Determine if a cycle exists
    while (fast !== null && fast.next !== null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) {
            // Cycle detected
            break;
        }
    }
    
    // If there is no cycle
    if (fast === null || fast.next === null) {
        return null;
    }

    // Phase 2: Find the start of the cycle
    slow = head;
    while (slow !== fast) {
        slow = slow.next;
        fast = fast.next;
    }

    // Both pointers are now at the start of the cycle
    return slow;
}
```

### Time and Space Complexity:
- **Time Complexity:** O(n) - We only traverse the linked list a few times.
- **Space Complexity:** O(1) - No additional storage is used.

