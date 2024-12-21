# [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Approaches
- [Two Pass Approach](#two-pass-approach)
- [One Pass Approach](#one-pass-approach)

### Two Pass Approach

In this approach, we do two iterations over the linked list:

1. **Calculate Length**: First, we traverse the entire list to calculate the total length of the linked list. 
2. **Identify Node to Remove**: With the total number of nodes in hand, we calculate the position of the node to be removed from the start of the list using the formula: `length - n + 1`.
3. Finally, remove the identified node.

#### Detailed Steps:
- Traverse the list to get its length.
- Calculate the index of the node to remove from the start.
- Traverse the list again to reach that node and remove it.

#### Implementation:

```go
type ListNode struct {
	Val  int
	Next *ListNode
}

func removeNthFromEnd(head *ListNode, n int) *ListNode {
	// Calculate the length of the list
	length := 0
	for current := head; current != nil; current = current.Next {
		length++
	}
	
	// Create a dummy node to simplify edge cases
	dummy := &ListNode{Next: head}
	current := dummy
	
	// Find the node right before the node to remove
	for i := 0; i < length-n; i++ {
		current = current.Next
	}
	
	// Remove the nth node from end by skipping it
	current.Next = current.Next.Next
	
	return dummy.Next
}
```

**Time Complexity**: O(L), where L is the length of the linked list (since we traverse the list twice).  
**Space Complexity**: O(1), as we're using a constant amount of extra space.

### One Pass Approach

This approach seeks to solve the problem in a single traversal using the two-pointer technique:

1. **Initialization**: Use two pointers, `first` and `second`, initialized at the head of the list. A `dummy` node is used to handle edge cases, such as removing the head of the list.
2. **Offset Pointers**: Move `first` to create a gap of `n` nodes between `first` and `second`.
3. **Traverse**: Move both pointers simultaneously until `first` reaches the end of the list.
4. **Remove Node**: At this point, `second` is one node before the node to be removed. Skip the next node to remove it.

#### Detailed Steps:
- Move the `first` n+1 steps ahead to maintain the gap (this ensures `second` is just before the node to remove).
- Move both pointers until `first` reaches the end.
- Modify the `next` pointer of `second` to skip the target node.

#### Implementation:

```go
type ListNode struct {
	Val  int
	Next *ListNode
}

func removeNthFromEnd(head *ListNode, n int) *ListNode {
	// Dummy node to handle edge cases cleanly
	dummy := &ListNode{Next: head}
	first, second := dummy, dummy
	
	// Move `first` pointer `n+1` steps ahead to maintain a gap of `n`
	for i := 0; i <= n; i++ {
		first = first.Next
	}
	
	// Move both pointers to the end, maintaining the gap
	for first != nil {
		first = first.Next
		second = second.Next
	}
	
	// Remove the nth node from end
	second.Next = second.Next.Next
	
	return dummy.Next
}
```

**Time Complexity**: O(L), where L is the length of the linked list (since we traverse the list once).  
**Space Complexity**: O(1), as we're using a constant amount of extra space. 

This one-pass approach is optimal in terms of time complexity while maintaining constant space, and is generally the preferred method for solving this problem.

