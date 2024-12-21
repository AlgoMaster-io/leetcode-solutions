# [Leetcode 92: Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

## Approaches
1. [Reverse Subsection with Array](#approach-1-reverse-subsection-with-array)
2. [Iterative Reverse In-place](#approach-2-iterative-reverse-in-place)

---

## Approach 1: Reverse Subsection with Array

### Intuition
The idea here is to first transform the linked list into an array which allows us to conveniently manipulate and reverse the subsection. We then build the linked list from this altered array. This method leverages the flexibility of arrays to handle subsets of data but doesn't take full advantage of the linked list structure.

### Detailed Steps:
1. Create a list from the linked list nodes to easily access elements via indices.
2. Reverse the desired portion of this list in place (between `left` and `right` indices).
3. Reconstruct the linked list from the modified list.

```javascript
function reverseBetween(head, left, right) {
    if (!head) return null;
    
    // Convert the linked list to array for easy manipulation
    let nodes = [];
    let current = head;
    while (current) {
        nodes.push(current);
        current = current.next;
    }
    
    // Reverse the particular section from left to right
    let start = left - 1, end = right - 1;
    while (start < end) {
        [nodes[start], nodes[end]] = [nodes[end], nodes[start]];
        start++;
        end--;
    }
    
    // Reconstruct the linked list from modified array
    for (let i = 0; i < nodes.length - 1; i++) {
        nodes[i].next = nodes[i + 1];
    }
    nodes[nodes.length - 1].next = null;
    
    return nodes[0];
}
```

**Time Complexity**: O(N), where N is the number of nodes in the linked list. We iterate through the list twice.  
**Space Complexity**: O(N), since we store all nodes in an array.

---

## Approach 2: Iterative Reverse In-place

### Intuition
Directly work with the linked list by carefully manipulating pointers. This approach is more memory-efficient as it doesn't need additional space. The key is to manage the pointers in a loop for the portion that needs reversing while maintaining other connections intact.

### Detailed Steps:
1. Traverse to just before the `left` position in the list.
2. Reverse the sublist using the classic sublist reversal technique.
3. Maintain pointers before and after the reversal to re-attach the reversed section back to the list.

```javascript
function reverseBetween(head, left, right) {
    if (!head) return null;

    // Dummy node to handle edge cases where head might change
    let dummyNode = new ListNode(0);
    dummyNode.next = head;
    
    // Step 1: Traverse to the node before 'left'
    let prev = dummyNode;
    for (let i = 1; i < left; i++) {
        prev = prev.next;
    }
    
    // Start of the sublist to be reversed
    let reverseStart = prev.next;
    let curr = reverseStart.next;
    
    // Step 2: Reverse the [left, right] range
    for (let i = 0; i < right - left; i++) {
        reverseStart.next = curr.next;
        curr.next = prev.next;
        prev.next = curr;
        curr = reverseStart.next;
    }
    
    return dummyNode.next;
}
```

**Time Complexity**: O(N), where N is the total number of nodes. It requires a single pass through.
**Space Complexity**: O(1), as the approach works in-place without extra data structures. 

By directly manipulating the pointers, Approach 2 is the most efficient both in terms of time and space, adhering to the constraints of in-place reversal within a linked list framework.

