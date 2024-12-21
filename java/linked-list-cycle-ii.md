# [Leetcode 141: Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

## Approaches:
- [Approach 1: Brute Force - HashSet](#approach-1-brute-force---hashset)
- [Approach 2: Floyd’s Cycle Detection Algorithm (Tortoise and Hare)](#approach-2-floyds-cycle-detection-algorithm-tortoise-and-hare)

### Approach 1: Brute Force - HashSet

**Intuition:**

One straightforward way to determine if there's a cycle in a linked list is to keep track of nodes we've already visited. If we encounter a node we've already seen, there's a cycle. This can be achieved using a `HashSet` where we store each visited node.

**Steps:**
1. Initialize a `HashSet` to keep track of visited nodes.
2. Traverse through the list starting from the head node.
3. For each node, check if it's already in the `HashSet`.
   - If yes, there is a cycle, and the node causing the cycle is the current node.
   - If no, add the node to the `HashSet` and continue.
4. If the traversal reaches the end of the list (`null`), then there is no cycle.

**Code:**

```java
import java.util.HashSet;

public class Solution {
    public ListNode detectCycle(ListNode head) {
        // Initialize a HashSet to keep track of visited nodes
        HashSet<ListNode> visited = new HashSet<>();
        
        ListNode current = head;
        // Traverse the list
        while (current != null) {
            // If we've already visited this node, cycle detected
            if (visited.contains(current)) {
                return current;
            }
            // Mark this node as visited
            visited.add(current);
            // Move to the next node
            current = current.next;
        }
        
        // If we reach here, there's no cycle
        return null;
    }
}
```

**Time Complexity:** `O(n)`, where `n` is the number of nodes in the linked list, because each node is visited at most once.

**Space Complexity:** `O(n)` due to the space required for storing visited nodes in the `HashSet`.

### Approach 2: Floyd’s Cycle Detection Algorithm (Tortoise and Hare)

**Intuition:**

The Tortoise and Hare algorithm is an optimized method to detect cycles using two pointers. One pointer (the tortoise) moves one step at a time while the other pointer (the hare) moves two steps at a time. If there is a cycle, the hare will eventually meet the tortoise within the cycle.

Once a cycle is detected, reset one pointer to the start of the list and keep the other pointer at the meeting point. Move both pointers one step at a time. The point where they meet again is the start of the cycle.

**Steps:**
1. Initialize two pointers, `slow` and `fast`.
2. Move `slow` by one step and `fast` by two steps in the list.
3. If they meet, a cycle exists. Otherwise, if `fast` or `fast.next` becomes null, there's no cycle.
4. To find the starting node of the cycle, reset `slow` to the head of the list and move both `slow` and `fast` one step at a time; the node where they meet is the start of the cycle.

**Code:**

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null) return null;
        
        // Initialize two pointers
        ListNode slow = head;
        ListNode fast = head;
        
        // Move the slow pointer by 1 and fast pointer by 2
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            
            // If they meet, a cycle exists
            if (slow == fast) {
                ListNode start = head;
                while (start != slow) {
                    slow = slow.next;
                    start = start.next;
                }
                return start;
            }
        }
        
        // No cycle present
        return null;
    }
}
```

**Time Complexity:** `O(n)`, as each node is visited at most twice.

**Space Complexity:** `O(1)`, since no additional data structures are used for storage.

