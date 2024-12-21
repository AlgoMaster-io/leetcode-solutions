# [Leetcode 160: Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Using HashSet](#using-hashset)
3. [Two-pointer Technique](#two-pointer-technique)

---

### Brute Force Approach

The simplest way to solve this problem is to use a nested loop to check each node in list A against every node in list B to find the intersection.

#### Intuition
For each node in list A, traverse list B and check if there's a node in B that matches the current node in A. If a matching node is found, that's the intersection node.

#### Code
```csharp
public class Solution {
    public ListNode GetIntersectionNode(ListNode headA, ListNode headB) {
        // Edge case, if any list is empty, no intersection
        if (headA == null || headB == null) return null;

        // Traverse each list A
        while (headA != null) {
            // For current node in list A, check all nodes in list B
            ListNode currentB = headB;
            while (currentB != null) {
                // Compare if nodes are the same (intersect)
                if (headA == currentB) {
                    return headA;
                }
                currentB = currentB.next;
            }
            headA = headA.next;
        }
        return null;
    }
}
```

#### Complexity
- **Time Complexity:** O(m * n), where m and n are the lengths of lists A and B respectively.
- **Space Complexity:** O(1), no additional space is used aside from variables.

---

### Using HashSet

Instead of using a nested loop, we can use a hash set to store nodes of one list and then check if any node of the other list is already in the set.

#### Intuition
We store all the nodes of list B in a hash set. Then, we iterate over list A and at each step, we check if the node exists in the set. If it does, that's the intersection node.

#### Code
```csharp
public class Solution {
    public ListNode GetIntersectionNode(ListNode headA, ListNode headB) {
        // Edge case, if any list is empty, no intersection
        if (headA == null || headB == null) return null;
        
        HashSet<ListNode> nodesInB = new HashSet<ListNode>();

        // Add all nodes of B to the set
        while (headB != null) {
            nodesInB.Add(headB);
            headB = headB.next;
        }

        // Traverse list A and check for intersection
        while (headA != null) {
            // If current node in A is in B, return intersection
            if (nodesInB.Contains(headA)) {
                return headA;
            }
            headA = headA.next;
        }

        // If no intersection found
        return null;
    }
}
```

#### Complexity
- **Time Complexity:** O(m + n), where m and n are the lengths of list A and list B.
- **Space Complexity:** O(n), we use the hash set to store nodes of list B.

---

### Two-pointer Technique

This approach uses two pointers to traverse the lists simultaneously to find the intersection point.

#### Intuition
The idea is to have two pointers initially set at the heads of A and B. Traverse them to the ends and then switch to the head of the other list. By the time they both traverse the combined length of lists A and B, a parallel traversal finds the intersection node due to their lengths being evened out by the switch.

#### Code
```csharp
public class Solution {
    public ListNode GetIntersectionNode(ListNode headA, ListNode headB) {
        // Edge case, if any list is empty, no intersection
        if (headA == null || headB == null) return null;

        ListNode pointerA = headA;
        ListNode pointerB = headB;

        // Traverse both lists
        while (pointerA != pointerB) {
            // If pointerA reaches the end, assign to headB, else move to next
            pointerA = pointerA == null ? headB : pointerA.next;
            // If pointerB reaches the end, assign to headA, else move to next
            pointerB = pointerB == null ? headA : pointerB.next;
        }

        // Either the intersection node or null
        return pointerA;
    }
}
```

#### Complexity
- **Time Complexity:** O(m + n), where m and n are the lengths of list A and list B. Each list is traversed at most twice.
- **Space Complexity:** O(1), no additional data structures are used.

This final approach is optimal both in terms of time complexity and space complexity, and is the recommended solution for its elegance and efficiency.

