# [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Approach 1: Brute Force (Basic)

### Solution
javascript
```javascript
// Time Complexity: O(m * n)
// Space Complexity: O(1)

function getIntersectionNode(headA, headB) {
    // Iterate through each node of list A
    while (headA !== null) {
        let temp = headB;

        // Check if it matches any node in list B
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

## Approach 2: Using HashSet (Intermediate)

### Solution
javascript
```javascript
// Time Complexity: O(m + n)
// Space Complexity: O(m) or O(n)

function getIntersectionNode(headA, headB) {
    const visitedNodes = new Set();

    // Add all nodes of list A to the set
    while (headA !== null) {
        visitedNodes.add(headA);
        headA = headA.next;
    }

    // Check for intersection in list B
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
javascript
```javascript
// Time Complexity: O(m + n)
// Space Complexity: O(1)

function getIntersectionNode(headA, headB) {
    if (headA === null || headB === null) {
        return null;
    }

    let pointerA = headA;
    let pointerB = headB;

    // Traverse both lists
    while (pointerA !== pointerB) {
        // Switch to the other list when reaching the end
        pointerA = (pointerA === null) ? headB : pointerA.next;
        pointerB = (pointerB === null) ? headA : pointerB.next;
    }

    return pointerA; // Either intersection node or null
}
```

