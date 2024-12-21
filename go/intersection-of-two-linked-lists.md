# [Leetcode 160: Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Hash Table](#approach-2-using-hash-table)
- [Approach 3: Two Pointers (Optimal Solution)](#approach-3-two-pointers-optimal-solution)

---

## Approach 1: Brute Force

### Intuition
The brute force solution is straightforward: for each node in the first linked list, iterate through the second list to check if the nodes are the same (i.e., have the same reference). If they point to the same node, an intersection is found.

### Implementation

```go
// Definition for singly-linked list.
type ListNode struct {
    Val int
    Next *ListNode
}

func getIntersectionNode(headA, headB *ListNode) *ListNode {
    // Iterate through each node in list A
    for a := headA; a != nil; a = a.Next {
        // For each node in list A, iterate through each node in list B
        for b := headB; b != nil; b = b.Next {
            // Check if nodes are the same (pointer comparison)
            if a == b {
                return a
            }
        }
    }
    return nil
}
```

### Time and Space Complexity
- **Time Complexity:** O(m * n), where `m` and `n` are the lengths of the two linked lists. Each node in list A is compared with every node in list B.
- **Space Complexity:** O(1), as no additional space is used.

---

## Approach 2: Using Hash Table

### Intuition
By using a hash table, we can store all nodes from the first list. Then, iterate through the second list to find the first node that appears in the hash table, which would be the intersection node.

### Implementation

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    nodesInA := make(map[*ListNode]bool)

    // Store all nodes in list A in a hash table
    for a := headA; a != nil; a = a.Next {
        nodesInA[a] = true
    }

    // Traverse list B to find the first common node
    for b := headB; b != nil; b = b.Next {
        if nodesInA[b] {
            return b
        }
    }
    return nil
}
```

### Time and Space Complexity
- **Time Complexity:** O(m + n), where `m` and `n` are the lengths of the two linked lists. We traverse both lists a single time each.
- **Space Complexity:** O(m), where `m` is the number of nodes in list A stored in the hash table.

---

## Approach 3: Two Pointers (Optimal Solution)

### Intuition
The optimal approach uses two pointers to traverse both linked lists. Initially, two pointers start at the heads of each list. Once a pointer reaches the end of a list, it switches to the head of the other list. By moving through both lists in this manner, the pointers will eventually align at the intersection point due to the equalized path achieved by both lists.

### Implementation

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    if headA == nil || headB == nil {
        return nil
    }
    
    a, b := headA, headB
    
    // Both pointers move through both lists
    for a != b {
        // When reaching the end of a list, switch to the head of the other list
        a = if a == nil { headB } else { a.Next }
        b = if b == nil { headA } else { b.Next }
    }
    
    return a // or b, both are the intersection node or nil if no intersection
}
```

### Time and Space Complexity
- **Time Complexity:** O(m + n), where `m` and `n` are the lengths of the two linked lists. Each node is traversed at most twice.
- **Space Complexity:** O(1), as the solution uses only constant extra space for the pointers.

This two-pointer approach is not only time-efficient but also space-efficient, making it a commonly recommended solution for this problem.

