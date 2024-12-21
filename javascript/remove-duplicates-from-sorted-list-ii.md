[Leetcode Problem 82: Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## Approaches

- [Approach 1: Dummy Head with Iterative Approach](#approach-1-dummy-head-with-iterative-approach)
- [Approach 2: Recursive Approach](#approach-2-recursive-approach)

---

### Approach 1: Dummy Head with Iterative Approach

#### Intuition
The core idea is to traverse the linked list while keeping track of duplicates using a dummy head. The dummy head helps to manage removals efficiently as it prevents losing the reference to the head of the list. We maintain a `prev` pointer, which always points to the node last confirmed as not a duplicate. As we move through the list, we check whether the current node is a start of a sequence of duplicates.

#### Detailed Steps
1. Initialize a dummy node that points to the head of the list. This helps manage edge cases where the head itself may need to be removed.
2. Use a `prev` pointer pointing to the dummy node and a `current` pointer pointing to the head.
3. Traverse through the list:
   - If `current` and `current.next` have the same value, it means we have found duplicates. Skip all nodes that have duplicate values by advancing `current` until the values differ.
   - If no duplicates were found, move the `prev` pointer to point to the current node.
   - Always advance the `current` pointer one step at a time.
4. After traversal, return `dummy.next` as it skips the dummy head.

#### Code

```javascript
function deleteDuplicates(head) {
    if (!head) return null;
    
    // Initialize a dummy node and set it before the head
    let dummy = new ListNode(0);
    dummy.next = head;
    
    // Previous node starts at dummy
    let prev = dummy;
    
    // Current node starts at head
    let current = head;
    
    while (current) {
        // Are we at the start of duplicates sublist
        // Move the current node to the end of the duplicates sublist
        if (current.next && current.val === current.next.val) {
            while (current.next && current.val === current.next.val) {
                current = current.next;
            }
            prev.next = current.next;
        } else {
            // Move prev pointer to the next non-duplicate node
            prev = prev.next;
        }
        // Move current node to the next in list
        current = current.next;
    }
    
    // Return the next of dummy node which is the head after removing duplicates
    return dummy.next;
}
```

#### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the linked list. Each node is visited and removed if needed.
- **Space Complexity**: O(1), as the solution uses a constant amount of extra space.

---

### Approach 2: Recursive Approach

#### Intuition
We can also solve the problem using recursion by systematically processing each node and excluding duplicates. For each recursive call, we decide whether to include the node in the result list or skip it based on whether it is part of a sequence of duplicates.

#### Detailed Steps
1. Define a recursive function that will take the current node as input.
2. If the current node is `null`, return `null`.
3. If the current node has duplicates:
   - Skip all nodes with duplicate values and call recursion for the next distinct node.
4. If the current node does not have duplicates:
   - Call recursion for the next node and link the current node to this recursive result.
5. Return the processed list from the dummy node.

#### Code

```javascript
function deleteDuplicates(head) {
    if (!head) return null;
    
    // If there's no next element, head is the end of the list, return it
    if (!head.next) return head;
    
    // If the current node is the start of duplicates
    if (head.val === head.next.val) {
        // Skip all nodes with the same value
        while (head.next && head.val === head.next.val) {
            head = head.next;
        }
        // Call recursion for the next distinct node
        return deleteDuplicates(head.next);
    } else {
        // Attach the processed list to current node if it's not a duplicate
        head.next = deleteDuplicates(head.next);
        return head;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of nodes in the linked list.
- **Space Complexity**: O(N), due to the recursion stack space in the worst case.

