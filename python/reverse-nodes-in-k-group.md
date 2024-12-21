# [Leetcode 25: Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

## Approaches:
- [Approach 1: Iterative Reverse (Basic)](#approach-1-iterative-reverse-basic)
- [Approach 2: Recursive Reverse (Optimized)](#approach-2-recursive-reverse-optimized)

### Approach 1: Iterative Reverse (Basic)

#### Intuition:
The problem involves reversing nodes of a linked list in groups of size `k`. The idea is to keep reversing nodes in sets of `k` until there are no more complete sets left. This iterative solution is straightforward as we move through the list group by group and reverse the nodes.

#### Solution:
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseKGroup(head, k):
    # Dummy node initialization to handle edge cases smoothly
    dummy = ListNode(0)
    dummy.next = head
    prev_group_end = dummy
    
    while True:
        # Use `kth` node helper to find the end of the next k-group
        kth = get_kth_node(prev_group_end, k)
        if not kth:
            break  # No more full k-groups present
        
        # Reverse the group
        group_start = prev_group_end.next
        next_group_start = kth.next
        prev, curr = kth.next, group_start
        
        # Reverse nodes in this group
        while curr != next_group_start:
            tmp = curr.next
            curr.next = prev
            prev = curr
            curr = tmp
        
        # Adjust pointers for the reversed group
        prev_group_end.next = kth
        prev_group_end = group_start
        
    return dummy.next

def get_kth_node(curr, k):
    # Helper to find the kth node in current sequence
    while curr and k > 0:
        curr = curr.next
        k -= 1
    return curr
```

#### Time and Space Complexities:
- **Time Complexity:** O(n) - Each node is visited and processed once.
- **Space Complexity:** O(1) - In-place reversal, constant extra space.

### Approach 2: Recursive Reverse (Optimized)

#### Intuition:
We can use recursion to directly reverse the nodes in the current `k` group and then proceed to the next group. Recursion can provide us cleaner and potentially more intuitive solutions for problems that involve iterative repetitive steps like this one.

#### Solution:
```python
def reverseKGroup(head, k):
    # Determining if there are enough nodes left to reverse
    count = 0
    ptr = head
    while count < k and ptr:
        ptr = ptr.next
        count += 1
    
    if count == k:
        # If we have at least k nodes, reverse them
        reversed_head = self.reverse(head, k)
        # Recur for the remaining nodes and connect the head to the reversed result
        head.next = self.reverseKGroup(ptr, k)
        return reversed_head
    
    return head

def reverse(head, k):
    # Reverse k nodes in-place
    prev = None
    curr = head
    while k > 0:
        next_node = curr.next
        curr.next = prev
        prev = curr
        curr = next_node
        k -= 1
    return prev
```

#### Time and Space Complexities:
- **Time Complexity:** O(n) - Each node is involved in a single reversal.
- **Space Complexity:** O(n/k) - Due to the recursion stack, where n/k is the number of complete groups.

Each approach has its advantages: the iterative solution provides direct control over the reversal process, while the recursive approach offers a clean and intuitive way to handle repeated reversal groups. Both solutions efficiently manage the constraints of the problem.

