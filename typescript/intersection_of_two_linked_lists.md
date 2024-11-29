# 160. [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(m * n)
// Space Complexity: O(1)
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val?: number, next?: ListNode | null) {
        this.val = val === undefined ? 0 : val;
        this.next = next === undefined ? null : next;
    }
}

function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
    while (headA !== null) {
        let temp = headB;

        while (temp !== null) {
            if (headA === temp) {
                return headA; // Intersection found
            }
            temp = temp.next;
        }

        headA = headA.next;
    }

    return null; // No intersection
}
```

## Approach 2: Using Set (Intermediate)

### Solution
typescript
```typescript
// Time Complexity: O(m + n)
// Space Complexity: O(m) or O(n)
function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
    const visitedNodes = new Set<ListNode>();

    while (headA !== null) {
        visitedNodes.add(headA);
        headA = headA.next;
    }

    while (headB !== null) {
        if (visitedNodes.has(headB)) {
            return headB; // Intersection found
        }
        headB = headB.next;
    }

    return null; // No intersection
}
```

## Approach 3: Two Pointers (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(m + n)
// Space Complexity: O(1)
function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
    if (headA === null || headB === null) {
        return null;
    }

    let pointerA = headA;
    let pointerB = headB;

    while (pointerA !== pointerB) {
        pointerA = (pointerA === null) ? headB : pointerA.next;
        pointerB = (pointerB === null) ? headA : pointerB.next;
    }

    return pointerA; // Either intersection node or null
}
```

