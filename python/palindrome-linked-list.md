# [Leetcode 234: Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

## Approaches
1. [Reverse and Compare](#approach-1-reverse-and-compare)
2. [Recursive Check](#approach-2-recursive-check)
3. [Fast and Slow Pointer (Optimal Solution)](#approach-3-fast-and-slow-pointer-optimal-solution)

## Approach 1: Reverse and Compare

### Intuition
To determine if a linked list is a palindrome, one straightforward method is to reverse the linked list and compare it to the original. If both the original and reversed lists are identical, then the linked list is a palindrome.

### Steps
1. **Copy the linked list** into an array.
2. **Reverse the array** without modifying the original linked list.
3. **Compare the original and reversed arrays** element-wise.
4. If all elements are identical, the linked list is a palindrome.

### Code
```python
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        vals = []
        currentNode = head
        
        # Convert linked list to array
        while currentNode is not None:
            vals.append(currentNode.val)
            currentNode = currentNode.next
        
        # Use two-pointer technique to check for palindrome
        left, right = 0, len(vals) - 1
        while left < right:
            if vals[left] != vals[right]:
                return False
            left, right = left + 1, right - 1
        
        return True
```

### Complexity
- **Time Complexity:** O(n), where n is the number of nodes in the linked list, as we traverse the entire list to copy and then compare.
- **Space Complexity:** O(n) due to the space used by the array.

## Approach 2: Recursive Check

### Intuition
In recursion, we move to the end of the linked list using the recursive call stack, and then start comparing elements when unwinding the stack. We can use a helper function with a pointer advancing in the recursion to compare elements.

### Steps
1. Use recursion to reach the end of the linked list.
2. As the recursion stack unwinds, compare the values from the front and back of the linked list.
3. Use a mutable structure to keep track of the front node to compare with the back node during the recursion unwind phase.

### Code
```python
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        self.front_pointer = head
        
        def recursively_check(current_node):
            if current_node is not None:
                if not recursively_check(current_node.next):
                    return False
                if self.front_pointer.val != current_node.val:
                    return False
                self.front_pointer = self.front_pointer.next
            
            return True
        
        return recursively_check(head)
```

### Complexity
- **Time Complexity:** O(n), because we traverse the whole linked list once.
- **Space Complexity:** O(n) due to recursive call stack usage.

## Approach 3: Fast and Slow Pointer (Optimal Solution)

### Intuition
The optimal solution leverages a fast-slow pointer technique. Using this strategy:
- The slow pointer moves one step at a time, while the fast pointer moves two steps. Thus, when the fast pointer reaches the end, the slow pointer will be at the midpoint.
- We reverse the second half of the list and then compare it with the first half.

### Steps
1. **Find the middle of the linked list** using the fast and slow technique.
2. **Reverse the second half** of the list starting from the middle.
3. **Compare the first half and the reversed second half** node by node.
4. Optionally, reverse the second half back to its original order to restore the list structure.

### Code
```python
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        if not head or not head.next:
            return True
        
        # Find the end of the first half and reverse second half
        def reverse_list(head):
            previous = None
            current = head
            while current is not None:
                next_node = current.next
                current.next = previous
                previous = current
                current = next_node
            return previous
        
        def end_of_first_half(head):
            fast = head
            slow = head
            while fast.next is not None and fast.next.next is not None:
                fast = fast.next.next
                slow = slow.next
            return slow
        
        end_of_first = end_of_first_half(head)
        start_of_second = reverse_list(end_of_first.next)
        
        # Check if palindrome
        p1 = head
        p2 = start_of_second
        result = True
        while result and p2 is not None:
            if p1.val != p2.val:
                result = False
            p1 = p1.next
            p2 = p2.next
        
        # Restore the list
        end_of_first.next = reverse_list(start_of_second)
        
        return result
```

### Complexity
- **Time Complexity:** O(n), where n is the number of nodes in the linked list.
- **Space Complexity:** O(1), since no additional data structures are needed, besides a few pointers.

