# [Leetcode 61: Rotate List](https://leetcode.com/problems/rotate-list/)

## Approaches

1. [Simple List Appending and Slicing](#simple-list-appending-and-slicing)
2. [Cycle-based Rotation](#cycle-based-rotation)
3. [Optimal Two-pointer Approach](#optimal-two-pointer-approach)

---

### 1. Simple List Appending and Slicing

**Intuition:**  
The naive approach would involve converting the list into an array format. We can append the list to itself and slice it to get the rotated list. This is straightforward but not very efficient due to the extra conversion needed and direct manipulation of list nodes.

**Steps:**

- Convert the linked list elements into an array.
- Duplicate the array to emulate rotation.
- Perform slicing based on the rotation value.
- Construct a new linked list from the rotated array.

```go
func rotateRight(head *ListNode, k int) *ListNode {
    if head == nil || head.Next == nil || k == 0 {
        return head
    }
    
    // Step 1: Convert linked list to array
    var arr []int
    length := 0
    current := head
    for current != nil {
        arr = append(arr, current.Val)
        current = current.Next
        length++
    }
    
    // Step 2: Determine the effective rotation
    k = k % length
    if k == 0 {
        return head
    }
    
    // Step 3: Append and slice the list array
    arr = append(arr, arr...)
    newHead := arr[length-k : 2*length-k]
    
    // Step 4: Convert array back to linked list
    dummy := &ListNode{}
    current = dummy
    for _, val := range newHead {
        current.Next = &ListNode{Val: val}
        current = current.Next
    }
    
    return dummy.Next
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

---

### 2. Cycle-based Rotation

**Intuition:**  
Instead of treating the list as an array, we can visualize it as a cycle or circular linked list. By connecting the last node to the first, we create a loop, and the problem reduces to finding the breaking point after rotating.

**Steps:**

- Compute the length of the linked list.
- Connect the last node to the head to form a circular linked list.
- Locate the new head after (length - k % length) steps.
- Break the cycle to form the rotated list.

```go
func rotateRight(head *ListNode, k int) *ListNode {
    if head == nil || head.Next == nil || k == 0 {
        return head
    }

    // Step 1: Close the linked list into a cycle
    length := 1
    oldTail := head
    for oldTail.Next != nil {
        oldTail = oldTail.Next
        length++
    }
    oldTail.Next = head
    
    // Step 2: Find new tail and new head
    k = k % length
    newTail := head
    for i := 0; i < length - k - 1; i++ {
        newTail = newTail.Next
    }
    newHead := newTail.Next
    
    // Step 3: Break the ring
    newTail.Next = nil
    
    return newHead
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

---

### 3. Optimal Two-pointer Approach

**Intuition:**  
Using a two-pointer approach provides a good balance between clarity and performance. Two pointers are used to determine where to cut the list so that it can be rotated correctly.

**Steps:**

- Compute the length of the linked list.
- Establish two pointers spaced `k % length` nodes apart.
- Move both pointers until the leading one reaches the end of the list.
- Perform the rotation by re-linking the necessary nodes.

```go
func rotateRight(head *ListNode, k int) *ListNode {
    if head == nil || head.Next == nil || k == 0 {
        return head
    }
    
    length := 1
    current := head
    for current.Next != nil {
        current = current.Next
        length++
    }
    
    k = k % length
    if k == 0 {
        return head
    }
    
    // Step 1: Find (length - k)th node
    slow, fast := head, head
    for i := 0; i < k; i++ {
        fast = fast.Next
    }
    for fast.Next != nil {
        slow = slow.Next
        fast = fast.Next
    }
    
    // Step 2: Reassign head and break the cycle
    newHead := slow.Next
    slow.Next = nil
    fast.Next = head
    
    return newHead
}
```

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

