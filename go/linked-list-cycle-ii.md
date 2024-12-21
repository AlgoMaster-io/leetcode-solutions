# [LeetCode 141: Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approaches
- [Approach 1: Hash Set](#approach-1-hash-set)
- [Approach 2: Floyd's Tortoise and Hare](#approach-2-floyds-tortoise-and-hare)

### Approach 1: Hash Set

**Intuition**:  
In this approach, we traverse the linked list and store each visited node in a hash set. If we encounter a node that is already in the set, it indicates the start of the cycle.

```go
// ListNode is a node in a singly linked list
type ListNode struct {
    Val  int
    Next *ListNode
}

func detectCycle(head *ListNode) *ListNode {
    // Create a map to store visited nodes
    visited := make(map[*ListNode]bool)

    // Traverse the linked list
    current := head
    for current != nil {
        // If the current node is already visited, it's the start of the cycle
        if visited[current] {
            return current
        }
        // Mark the current node as visited
        visited[current] = true
        // Move to the next node
        current = current.Next
    }
    // No cycle found
    return nil
}
```

**Time Complexity**: O(n)  
We traverse the linked list at most twice, where `n` is the number of nodes.

**Space Complexity**: O(n)  
We may potentially store each node in the hash set.

---

### Approach 2: Floyd's Tortoise and Hare

**Intuition**:  
This is an optimized solution using two pointers - the tortoise and the hare. We first check if a cycle exists by trying to find if they meet. If the cycle exists, we start the hare from the head of the linked list and move both pointers one step at a time until they meet again. The meeting point is the start of the cycle.

```go
func detectCycle(head *ListNode) *ListNode {
    if head == nil {
        return nil
    }

    tortoise, hare := head, head
    // Move tortoise one step and hare two steps
    for hare != nil && hare.Next != nil {
        tortoise = tortoise.Next
        hare = hare.Next.Next
        // Check if they meet
        if tortoise == hare {
            // Cycle detected, find the start of the cycle
            hare = head
            while hare != tortoise {
                hare = hare.Next
                tortoise = tortoise.Next
            }
            // hare and tortoise are now both at the start of the cycle
            return hare
        }
    }
    // No cycle found
    return nil
}
```

**Time Complexity**: O(n)  
In the worst case, the hare and tortoise pointers will traverse the linked list more than once but still linear relative to `n`.

**Space Complexity**: O(1)  
This approach uses constant space, only storing pointers to nodes.

