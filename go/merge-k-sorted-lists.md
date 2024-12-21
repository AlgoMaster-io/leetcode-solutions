# [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Min-Heap Approach](#min-heap-approach)
3. [Divide and Conquer Approach](#divide-and-conquer-approach)

### Brute Force Approach

**Intuition:**

The simplest approach is to collect all the nodes from the k linked lists into a list, sort this list, and then reconstruct the linked list. This approach is straightforward and ensures the correct outcome but is not efficient due to sorting overhead.

**Steps:**

1. Traverse each linked list and append all the nodes into a single list.
2. Sort this list based on the node values.
3. Create a new sorted linked list from the sorted node list.

**Implementation:**

```go
type ListNode struct {
    Val  int
    Next *ListNode
}

func mergeKLists(lists []*ListNode) *ListNode {
    nodes := []*ListNode{}
    // Step 1: Collect all nodes from the lists
    for _, list := range lists {
        for list != nil {
            nodes = append(nodes, list)
            list = list.Next
        }
    }
    
    // Step 2: Sort the nodes by value
    sort.Slice(nodes, func(i, j int) bool {
        return nodes[i].Val < nodes[j].Val
    })
    
    // Step 3: Reconstruct the sorted linked list
    dummy := &ListNode{} 
    current := dummy
    for _, node := range nodes {
        current.Next = node
        current = current.Next
    }
    
    // Set the last node's next to nil
    if current != nil {
        current.Next = nil
    }
    
    return dummy.Next
}
```

**Time Complexity:** O(N log N) where N is the total number of nodes. This comes from sorting the list of nodes.

**Space Complexity:** O(N) to store the nodes.

### Min-Heap Approach

**Intuition:**

A more efficient approach utilizes a min-heap (priority queue). By pushing the first node of each list into a heap, we can efficiently get the node with the smallest value at each step and continue merging lists.

**Steps:**

1. Define a min-heap that stores nodes based on their value.
2. Initialize the heap with the head node of each list.
3. Pop nodes from the heap and add them to the merged list. If a node has a next node, push it into the heap.

**Implementation:**

```go
import "container/heap"

type ListNode struct {
    Val  int
    Next *ListNode
}

// An alias for min-heap based on ListNode
type MinHeap []*ListNode

func (mh MinHeap) Len() int { return len(mh) }
func (mh MinHeap) Less(i, j int) bool { return mh[i].Val < mh[j].Val }
func (mh MinHeap) Swap(i, j int) { mh[i], mh[j] = mh[j], mh[i] }

func (mh *MinHeap) Push(x interface{}) {
    *mh = append(*mh, x.(*ListNode))
}

func (mh *MinHeap) Pop() interface{} {
    old := *mh
    n := len(old)
    x := old[n-1]
    *mh = old[0 : n-1]
    return x
}

func mergeKLists(lists []*ListNode) *ListNode {
    h := &MinHeap{}
    heap.Init(h)
    
    // Step 1: Push the head of each list into the heap
    for _, list := range lists {
        if list != nil {
            heap.Push(h, list)
        }
    }
    
    dummy := &ListNode{}
    current := dummy
    
    // Step 2: Iterate over the heap
    for h.Len() > 0 {
        // Extract the smallest node
        minNode := heap.Pop(h).(*ListNode)
        current.Next = minNode
        current = current.Next
        
        // If there is a next node, push it into the heap
        if minNode.Next != nil {
            heap.Push(h, minNode.Next)
        }
    }
    
    return dummy.Next
}
```

**Time Complexity:** O(N log k) where N is the total number of nodes and k is the number of linked lists. Logarithmic term comes from heap operations.

**Space Complexity:** O(k) for storing nodes in the heap.

### Divide and Conquer Approach

**Intuition:**

The divide and conquer strategy splits the k lists into pairs and merges each pair recursively. This reduces the problem to merging two lists at a time, akin to merging in merge sort, and gradually builds up the solution using recursion.

**Steps:**

1. If there's only one list, return it.
2. Divide the list into two halves and recursively merge each half.
3. Use a helper function to merge two sorted lists and continue recursively.

**Implementation:**

```go
type ListNode struct {
    Val  int
    Next *ListNode
}

func mergeTwoLists(l1, l2 *ListNode) *ListNode {
    dummy := &ListNode{}
    current := dummy
    // Merge two lists
    for l1 != nil && l2 != nil {
        if l1.Val < l2.Val {
            current.Next = l1
            l1 = l1.Next
        } else {
            current.Next = l2
            l2 = l2.Next
        }
        current = current.Next
    }
    // Append any remaining nodes
    if l1 != nil {
        current.Next = l1
    }
    if l2 != nil {
        current.Next = l2
    }
    return dummy.Next
}

func mergeKLists(lists []*ListNode) *ListNode {
    if len(lists) == 0 {
        return nil
    }
    if len(lists) == 1 {
        return lists[0]
    }
    mid := len(lists) / 2
    left := mergeKLists(lists[:mid])
    right := mergeKLists(lists[mid:])
    return mergeTwoLists(left, right)
}
```

**Time Complexity:** O(N log k), where N is the total number of nodes and k is the number of lists. Similar to merge sort.

**Space Complexity:** O(log k) due to the recursion stack used in the divide and conquer strategy.

