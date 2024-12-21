# [Leetcode 86: Partition List](https://leetcode.com/problems/partition-list/)

## Approaches
- [Approach 1: Two Pointers with O(n) Time and O(1) Space Complexity](#approach-1)

## Approach 1: Two Pointers with O(n) Time and O(1) Space Complexity

### Intuition
The core idea is to efficiently partition the given linked list into two separate lists: one containing all nodes with values less than `x` and the other containing nodes with values greater than or equal to `x`. We'll then link these two lists together. We will use two pointers, `less` and `greater`, to construct these two lists.

- Use a `less` list to keep track of the nodes with values less than `x`.
- Use a `greater` list to keep track of the nodes with values greater than or equal to `x`.
- Traverse the entire list once, moving each node to its respective ‘less’ or ‘greater’ list.
- Finally, concatenate the `less` list with the `greater` list.

### Steps
1. Create two dummy nodes: `less_head` and `greater_head`. These will be the starting nodes of our two lists.
2. Maintain two pointers: `less` and `greater`, initialized to point to `less_head` and `greater_head`, respectively.
3. Iterate over the original linked list:
   - If the current node's value is less than `x`, append it to the `less` list.
   - Otherwise, append it to the `greater` list.
4. After processing all nodes, concatenate the `less` list with the `greater` list by setting `less.next` to `greater_head.next`.
5. Ensure the end of the `greater` list points to `null` to avoid looping.
6. The partitioned list starts at `less_head.next` (since `less_head` is a dummy node).

### Code

```typescript
class ListNode {
    val: number;
    next: ListNode | null;
    constructor(val: number, next: ListNode | null = null) {
        this.val = val;
        this.next = next;
    }
}

function partition(head: ListNode | null, x: number): ListNode | null {
    // Dummy nodes to form the start of the less and greater lists
    let less_head = new ListNode(0);
    let greater_head = new ListNode(0);

    // Pointers to the end of the less and greater lists
    let less = less_head;
    let greater = greater_head;

    // Iterate through the list
    while (head !== null) {
        if (head.val < x) {
            // Append to less list
            less.next = head;
            less = less.next;
        } else {
            // Append to greater list
            greater.next = head;
            greater = greater.next;
        }
        // Move to the next node
        head = head.next;
    }

    // Ensure the greater list ends properly
    greater.next = null;

    // Connect the less list with the greater list
    less.next = greater_head.next;

    // Return the partitioned list, skipping the dummy node
    return less_head.next;
}
```

### Time Complexity
- **O(n)**, where n is the number of nodes in the linked list. We traverse each node once.

### Space Complexity
- **O(1)**, since we only use a constant amount of extra space for pointers and dummy nodes. The list is rearranged in-place.

This approach efficiently partitions the list with a single pass and minimal overhead, leveraging the two-pointer technique to manage and concatenate the linked lists.

