# [Leetcode 82: Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## Approaches
1. [Basic Approach: Two-Pointer with Extra Space](#approach-1-basic-approach-two-pointer-with-extra-space)
2. [Optimized Approach: In-Place Modification with Sentinel Node](#approach-2-optimized-approach-in-place-modification-with-sentinel-node)

## Approach 1: Basic Approach: Two-Pointer with Extra Space

### Intuition
In this approach, we use a hash map to store the frequency of each element in the given linked list. We then traverse the linked list again, constructing a new list with only the nodes that occur once. This approach is straightforward but requires extra space to maintain the frequency of each element.

```typescript
class ListNode {
    val: number
    next: ListNode | null
    constructor(val?: number, next?: ListNode | null) {
        this.val = (val===undefined ? 0 : val)
        this.next = (next===undefined ? null : next)
    }
}

function deleteDuplicates(head: ListNode | null): ListNode | null {
    const map: Map<number, number> = new Map();

    // First pass to count occurrences of each number
    let current = head;
    while (current !== null) {
        map.set(current.val, (map.get(current.val) || 0) + 1);
        current = current.next;
    }
    
    // Dummy node to help with edge cases
    const dummy = new ListNode(0);
    let newCurrent = dummy;
    current = head;

    // Second pass to link non-duplicate values
    while (current !== null) {
        if (map.get(current.val) === 1) {
            newCurrent.next = new ListNode(current.val);
            newCurrent = newCurrent.next;
        }
        current = current.next;
    }
    
    return dummy.next;
}
```

**Time Complexity**: O(n), where n is the number of nodes in the list. We traverse the list twice.

**Space Complexity**: O(n), as we use extra space to store frequencies in a map.

## Approach 2: Optimized Approach: In-Place Modification with Sentinel Node

### Intuition
For optimal space efficiency, we can perform the operation in-place. By using a sentinel node, we can easily handle the edge cases where the first few nodes might need to be removed. The key idea is to use two pointers: one for traversing the list and the other for linking the nodes that are not duplicates.

```typescript
function deleteDuplicates(head: ListNode | null): ListNode | null {
    // Sentinel node to easily handle head operation
    const sentinel = new ListNode(0);
    sentinel.next = head;
    
    let pred = sentinel;  // Predecessor, to attach the last non-removable node
    
    while (head !== null) {
        // If it's a beginning of duplicates, skip all duplicates
        if (head.next !== null && head.val === head.next.val) {
            // Move till the end of duplicates sublist
            while (head.next !== null && head.val === head.next.val) {
                head = head.next;
            }
            // Skip all duplicates
            pred.next = head.next; 
        } else {
            // Otherwise, move predecessor
            pred = pred.next;
        }
        // Move forward
        head = head.next;
    }
    
    return sentinel.next;
}
```

**Time Complexity**: O(n), because we traverse the list once.

**Space Complexity**: O(1), since we only use a constant amount of extra space.

