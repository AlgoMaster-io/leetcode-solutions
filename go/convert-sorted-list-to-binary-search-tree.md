# [Leetcode 109: Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)

## Approaches
- [Approach 1: Brute Force with Array Conversion](#approach-1)
- [Approach 2: Optimized Recursive In-place Conversion](#approach-2)

## Approach 1: Brute Force with Array Conversion

### Intuition
The simplest way to convert a sorted linked list to a BST is to first convert the linked list to an array. Given a sorted array, it's straightforward to construct a balanced BST by selecting the middle element as the root, and recursively applying the same logic to the left and right subarrays. 

### Detailed Explanation
1. **Convert List to Array**: Traverse the linked list and store the values in an array.
2. **Recursive BST Construction**: Use the median of the array as root and apply this recursively to the subarrays to ensure balance.

This approach simplifies the problem by leveraging the properties of balanced BST construction from sorted arrays.

### Code
```go
type ListNode struct {
	Val  int
	Next *ListNode
}

type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}

func sortedListToBST(head *ListNode) *TreeNode {
	// Convert the linked list to an array.
	var nums []int
	for head != nil {
		nums = append(nums, head.Val)
		head = head.Next
	}

	// Recursively convert the sorted array to a BST.
	return sortedArrayToBST(nums)
}

func sortedArrayToBST(nums []int) *TreeNode {
	if len(nums) == 0 {
		return nil
	}

	mid := len(nums) / 2
	root := &TreeNode{Val: nums[mid]}
	// Recursively construct the left and right subtrees.
	root.Left = sortedArrayToBST(nums[:mid])
	root.Right = sortedArrayToBST(nums[mid+1:])

	return root
}
```

### Time Complexity
- **Time**: O(n), where n is the number of nodes in the linked list. This is because we traverse the list once to form the array and then for array-to-BST conversion, each node is visited once.
- **Space**: O(n) for using an additional array to store the list elements.

## Approach 2: Optimized Recursive In-place Conversion

### Intuition
Instead of converting the list to an array, which requires O(n) extra space, we can directly build the BST using the in-order traversal property. We can use the two-pointers technique (using `slow` and `fast` pointers) to find the middle element of the linked list, which will become the root of the BST.

### Detailed Explanation
1. **Find Middle Node**: Use `slow` and `fast` pointers to find the middle node of the linked list. The middle node will be the root node of the BST.
2. **Break Linked List**: Disconnect the linked list from the middle node, forming two halves.
3. **Recursively Construct BST**: Repeat the above steps for the left and right sublists.

This leverages the halves technique directly on the linked list without converting it into an array, saving space.

### Code
```go
func sortedListToBST(head *ListNode) *TreeNode {
	if head == nil {
		return nil
	}
	if head.Next == nil {
		return &TreeNode{Val: head.Val}
	}

	// Find the middle of the linked list.
	slow, fast := head, head
	var prev *ListNode
	for fast != nil && fast.Next != nil {
		prev = slow
		slow = slow.Next
		fast = fast.Next.Next
	}

	// Disconnect the left half from the mid node.
	if prev != nil {
		prev.Next = nil
	}

	// Use the slow node as the root to maintain balanced BST.
	root := &TreeNode{Val: slow.Val}
	// Recursively construct the left and right subtrees.
	root.Left = sortedListToBST(head)      // Elements before slow
	root.Right = sortedListToBST(slow.Next) // Elements after slow

	return root
}
```

### Time Complexity
- **Time**: O(n log n), where n is the number of nodes in the linked list. Each level of recursion performs a linear traversal to find the middle element.
- **Space**: O(log n) due to the recursion stack required for balanced BST construction. Each recursive call contributes to the stack depth.  

This optimized solution keeps the space usage minimal while still efficiently constructing the BST in-place.

