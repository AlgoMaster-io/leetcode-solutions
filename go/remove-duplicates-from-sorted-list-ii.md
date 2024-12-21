# [Leetcode Problem 82: Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## Approaches
- [Naive Approach](#naive-approach)
- [Optimized Approach using a Dummy Node](#optimized-approach-using-a-dummy-node)

### Naive Approach

#### Intuition
The naive approach involves using a map to store the frequency of each node value. After counting the occurrences, we iterate through the list again and construct a new list containing only nodes with a frequency of one (i.e., nodes that are not duplicates).

#### Steps
1. Traverse the list to count the occurrences of each node's value using a hashmap.
2. Create a new dummy node which helps in easily managing edge cases.
3. Traverse the original list again and attach nodes with values that appear only once to the new list.
4. Return the next of the dummy node as it points to the beginning of the constructed list.

#### Time Complexity
- O(n): We traverse the list twice, so the time complexity is linear.

#### Space Complexity
- O(n): We use a map to store frequencies of each node value.

```go
func deleteDuplicates(head *ListNode) *ListNode {
    // Map to store the frequency of each node value
    frequency := make(map[int]int)
    
    // First pass to count the frequency of each node value
    current := head
    for current != nil {
        frequency[current.Val]++
        current = current.Next
    }
    
    // Create a dummy node to ease list construction
    dummy := &ListNode{}
    tail := dummy // Tail of the new list
    
    // Second pass to build the list with unique elements only
    current = head
    for current != nil {
        if frequency[current.Val] == 1 {
            // Attach the node with unique value to the new list
            tail.Next = &ListNode{Val: current.Val}
            tail = tail.Next
        }
        current = current.Next
    }
    
    return dummy.Next
}
```

### Optimized Approach using a Dummy Node

#### Intuition
Instead of using extra space to store frequencies, we directly manipulate the links. By leveraging a dummy node, we can easily manage edge cases where the first few nodes are duplicates.

#### Steps
1. Create a dummy node pointing to the head of the list.
2. Use a pointer `prev` initialized to dummy node and another pointer `current` to traverse the list.
3. For each sequence of nodes with the same value, if the length of the sequence is more than one, adjust the `next` pointer of `prev` to skip this sequence. Otherwise, move the `prev` pointer forward.
4. After finishing the traversal, `dummy.Next` will point to the head of the modified list without duplicates.

#### Time Complexity
- O(n): We traverse the list only once.

#### Space Complexity
- O(1): No extra space is used apart from a few pointers.

```go
func deleteDuplicates(head *ListNode) *ListNode {
    // Dummy node initiation
    dummy := &ListNode{Next: head}
    prev := dummy // prev initially points to dummy
    
    current := head // current is used to traverse

    for current != nil && current.Next != nil {
        if current.Val == current.Next.Val {
            // Detect duplicates by comparing current and next node values
            duplicateVal := current.Val
            // Skip all nodes with the same value
            for current != nil && current.Val == duplicateVal {
                current = current.Next
            }
            // Set prev's next to point to the node after duplicates
            prev.Next = current
        } else {
            // Only move the prev pointer if no duplicates were found
            prev = prev.Next
            current = current.Next
        }
    }
    
    return dummy.Next
}
```

This code efficiently removes duplicates in a sorted linked list by restructuring pointers directly, improving the overall space complexity compared to the naive method.

