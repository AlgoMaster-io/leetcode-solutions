[Leetcode Problem: Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

### Approaches
1. [Basic Iterative Approach](#basic-iterative-approach)
2. [Recursive Approach](#recursive-approach)
3. [Optimized Iterative Approach](#optimized-iterative-approach)

---

## Basic Iterative Approach

The basic idea is to add corresponding digits of the two numbers starting from the least significant digit (the head of the list). We use a carry variable to account for sums greater than or equal to 10.

### Intuition
- Traverse both linked lists simultaneously, digit by digit.
- For each pair of digits (one from each list), add them together along with any carry from the previous addition.
- If the sum is 10 or more, set the carry to 1, otherwise, reset it to 0.
- Continue until both lists and any remaining carry have been processed.

### Code
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def addTwoNumbers(l1: ListNode, l2: ListNode) -> ListNode:
    dummy_head = ListNode(0)  # Dummy head to facilitate result list creation
    current = dummy_head
    carry = 0

    while l1 or l2:  # Continue while either list is still non-empty
        # Get the value from each list, or 0 if the list is exhausted
        x = l1.val if l1 else 0
        y = l2.val if l2 else 0
        sum_value = carry + x + y  # Calculate sum plus carry

        # Update the carry for the next iteration
        carry = sum_value // 10
        current.next = ListNode(sum_value % 10)  # Create new node with the computed digit
        
        # Move pointers forward
        current = current.next
        
        # Move l1 and l2 pointers to the next node
        if l1:
            l1 = l1.next
        if l2:
            l2 = l2.next

    # Check if a carry is remaining after loop has concluded
    if carry:
        current.next = ListNode(carry)

    return dummy_head.next  # Dummy head used, head points to next
```

### Time and Space Complexity
- **Time Complexity**: O(max(m, n)), where m and n are the lengths of the two linked lists.
- **Space Complexity**: O(max(m, n)), the space used for the resulting linked list which is the maximum of m and n plus possibly one additional node for a carry at the end.

---

## Recursive Approach

Instead of iterating through the linked lists, we process each pair of nodes recursively.

### Intuition
- This approach mirrors the addition process, tackling it one node at a time by diving deeper into the lists recursively.
- Each recursion handles one pair of nodes, computes the sum, handles the carry, and then sets up the next recursive call for the next nodes.

### Code
```python
def addTwoNumbersRecursive(l1: ListNode, l2: ListNode, carry: int = 0) -> ListNode:
    # Base case: if we've reached the end of both lists and there's no carry, we're done.
    if not l1 and not l2 and not carry:
        return None

    # Calculate value for this node
    l1_value = l1.val if l1 else 0
    l2_value = l2.val if l2 else 0
    sum_value = l1_value + l2_value + carry  # Sum of corresponding nodes and carry

    # Prepare the result node with the last digit of the sum calculation
    current = ListNode(sum_value % 10)
    
    # Recurse for the next nodes, carrying over the tens place value (if any)
    current.next = addTwoNumbersRecursive(l1.next if l1 else None, l2.next if l2 else None, sum_value // 10)
    return current
```

### Time and Space Complexity
- **Time Complexity**: O(max(m, n)), because we recursively process each node.
- **Space Complexity**: O(max(m, n)), due to the recursion stack and the space for the new linked list.

---

## Optimized Iterative Approach

While the first iterative approach is already efficient, this variant uses fewer conditional checks by utilizing more Pythonic tools such as the zip function in conjunction with itertools.

### Intuition
- Use a loop to iterate through nodes using zip_longest, which stops when the longest list ends, and fill missing numbers with 0.
- This minimizes checks inside the loop and directly processes the nodes.

### Code
```python
from itertools import zip_longest

def addTwoNumbersOptimized(l1: ListNode, l2: ListNode) -> ListNode:
    dummy_head = ListNode()
    current = dummy_head
    carry = 0

    for a, b in zip_longest(iterate_list(l1), iterate_list(l2), fillvalue=0):
        total = a + b + carry
        carry, value = divmod(total, 10)
        current.next = ListNode(value)
        current = current.next

    if carry:
        current.next = ListNode(carry)

    return dummy_head.next

def iterate_list(node):
    while node:
        yield node.val
        node = node.next
```

### Time and Space Complexity
- **Time Complexity**: O(max(m, n))
- **Space Complexity**: O(max(m, n))

This problem is a classic example highlighting the elegance of linked lists in representing numbers, and these approaches demonstrate both the simplicity (iteratively) and elegance (recursively) of solving such addition.

