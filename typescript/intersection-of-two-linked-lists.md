## [Leetcode 160: Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

### Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Hash Set Approach](#hash-set-approach)
3. [Two Pointers Approach](#two-pointers-approach)

---

### Brute Force Approach

The brute force solution involves comparing each node of the first linked list with each node of the second linked list. 

#### Intuition
- For each node `a` in list A, traverse every node `b` in list B to see if they are the same (by reference).
- This approach guarantees finding the intersection node, but it does so inefficiently.

#### TypeScript Code

```typescript
class ListNode {
  val: number;
  next: ListNode | null;
  
  constructor(val?: number, next?: ListNode | null) {
    this.val = (val === undefined ? 0 : val);
    this.next = (next === undefined ? null : next);
  }
}

function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
  let a = headA;
  
  while (a !== null) {
    let b = headB;
    while (b !== null) {
      // Check if 'a' and 'b' point to the same node by reference
      if (a === b) {
        return a;
      }
      b = b.next;
    }
    a = a.next;
  }
  return null;
}
```

#### Complexity Analysis
- **Time Complexity**: O(m * n), where m and n are the lengths of the lists.
- **Space Complexity**: O(1), no extra space is used apart from variables.

---

### Hash Set Approach

By using a Hash Set, we can store all nodes of one list and then check if any node of the other list exists in the set.

#### Intuition
- Traverse List A and add each node to a hash set.
- Then traverse List B, checking each node against the set. If a node from List B exists in the set, that's the intersection node.

#### TypeScript Code

```typescript
function getIntersectionNodeUsingSet(headA: ListNode | null, headB: ListNode | null): ListNode | null {
  const nodesSeen = new Set<ListNode>();
  let a = headA;

  // Store all nodes of list A into the set
  while (a !== null) {
    nodesSeen.add(a);
    a = a.next;
  }

  let b = headB;
  // Check nodes of list B
  while (b !== null) {
    if (nodesSeen.has(b)) {
      return b; // Intersection found
    }
    b = b.next;
  }
  
  return null; // No intersection
}
```

#### Complexity Analysis
- **Time Complexity**: O(m + n), where m and n are the lengths of the lists.
- **Space Complexity**: O(m), depends on the length of List A as we store its nodes in a set.

---

### Two Pointers Approach

Two pointers method involves leveraging the difference in lengths of the two lists.

#### Intuition
- Use two pointers initially set to the heads of the given lists.
- Traverse until both pointers are the same. If one pointer reaches the end, reset it to the head of the other list.
- When both pointers traverse the same total length, they either meet at the intersection node or both reach null (ends), concluding there's no intersection.

#### TypeScript Code

```typescript
function getIntersectionNodeTwoPointers(headA: ListNode | null, headB: ListNode | null): ListNode | null {
  if (headA === null || headB === null) {
    return null;
  }

  let a = headA;
  let b = headB;

  // When either 'a' or 'b' reaches end they are reset to other list's head
  while (a !== b) {
    a = a === null ? headB : a.next;
    b = b === null ? headA : b.next;
  }
  
  // Either both pointers have met at the intersection node or both are null
  return a;
}
```

#### Complexity Analysis
- **Time Complexity**: O(m + n), where m and n are the lengths of the lists.
- **Space Complexity**: O(1), no additional space is used other than the pointer variables.

This two-pointer approach is elegant and optimal as it ensures both pointers traverse the same relative lengths by the end of the traversal, effectively balancing out any initial length disparity without requiring extra space.

