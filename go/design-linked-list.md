# [Leetcode 707: Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Approaches
- [Singly Linked List Using Arrays](#singly-linked-list-using-arrays)
- [Singly Linked List Using Pointers](#singly-linked-list-using-pointers)
- [Doubly Linked List Using Pointers](#doubly-linked-list-using-pointers)

## Approach 1: Singly Linked List Using Arrays

### Intuition
The idea is to use an array to simulate a linked list, where each element of the array is a node containing a value and an index pointing to the next node. This approach is more of a conceptual exercise to visualize how linked lists work with static arrays. It's not the most optimal or commonly used approach for linked list implementation.

### Implementation

```go
type MyLinkedList struct {
    nodes   [1000]int // let's assume max size is 1000 for simplicity
    next    [1000]int
    headIdx int
    size    int
}

// Initialize the linked list
func Constructor() MyLinkedList {
    list := MyLinkedList{
        headIdx: -1, // indicates that the list is empty
    }
    for i := 0; i < 1000; i++ {
        list.next[i] = -1 // there is no valid next index yet
    }
    return list
}

// Get the value at the index-th node in the linked list. If the index is invalid, return -1.
func (this *MyLinkedList) Get(index int) int {
    if index >= this.size || index < 0 {
        return -1
    }
    currIdx := this.headIdx
    for i := 0; i < index; i++ {
        currIdx = this.next[currIdx]
    }
    return this.nodes[currIdx]
}

// Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
func (this *MyLinkedList) AddAtHead(val int) {
    if this.size >= 1000 {
        return // no space left
    }
    newIdx := this.size
    this.nodes[newIdx] = val
    this.next[newIdx] = this.headIdx
    this.headIdx = newIdx
    this.size++
}

// Add a node of value val as the last element of the linked list.
func (this *MyLinkedList) AddAtTail(val int) {
    if this.size >= 1000 {
        return // no space left
    }
    newIdx := this.size
    this.nodes[newIdx] = val
    if this.headIdx == -1 {
        this.headIdx = newIdx
    } else {
        // find the last node
        currIdx := this.headIdx
        for this.next[currIdx] != -1 {
            currIdx = this.next[currIdx]
        }
        this.next[currIdx] = newIdx
    }
    this.size++
}

// Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted.
func (this *MyLinkedList) AddAtIndex(index int, val int) {
    if index > this.size || this.size >= 1000 {
        return
    }
    if index == 0 {
        this.AddAtHead(val)
        return
    }
    newIdx := this.size
    this.nodes[newIdx] = val
    currIdx := this.headIdx
    for i := 0; i < index-1; i++ {
        currIdx = this.next[currIdx]
    }
    // insert the new node
    this.next[newIdx] = this.next[currIdx]
    this.next[currIdx] = newIdx
    this.size++
}

// Delete the index-th node in the linked list, if the index is valid.
func (this *MyLinkedList) DeleteAtIndex(index int) {
    if index >= this.size || index < 0 {
        return
    }
    if index == 0 {
        this.headIdx = this.next[this.headIdx]
    } else {
        currIdx := this.headIdx
        for i := 0; i < index-1; i++ {
            currIdx = this.next[currIdx]
        }
        // skip the current node
        this.next[currIdx] = this.next[this.next[currIdx]]
    }
    this.size--
}
```

### Complexity
- **Time Complexity**: 
  - `Get`: O(n) - requires walking through the list to find the node.
  - `AddAtHead`: O(1) - because we directly add at the head.
  - `AddAtTail`: O(n) - similar reason as for `Get`.
  - `AddAtIndex`: O(n) - walk through to find the position to insert.
  - `DeleteAtIndex`: O(n) - walk through to find the position to delete.
- **Space Complexity**: O(1) - the space is fixed and does not grow with inputs.

## Approach 2: Singly Linked List Using Pointers

### Intuition
A singly linked list is a collection of nodes where each node contains a value and a pointer/reference to the next node in the list. We use a simple structure with pointers to allow nodes to point to subsequent nodes.

### Implementation

```go
type Node struct {
    val  int
    next *Node
}

type MyLinkedList struct {
    head *Node
    size int
}

func Constructor() MyLinkedList {
    return MyLinkedList{}
}

func (this *MyLinkedList) Get(index int) int {
    if index < 0 || index >= this.size {
        return -1
    }
    curr := this.head
    for i := 0; i < index; i++ {
        curr = curr.next
    }
    return curr.val
}

func (this *MyLinkedList) AddAtHead(val int) {
    this.head = &Node{val, this.head}
    this.size++
}

func (this *MyLinkedList) AddAtTail(val int) {
    newNode := &Node{val, nil}
    if this.size == 0 {
        this.head = newNode
    } else {
        curr := this.head
        for curr.next != nil {
            curr = curr.next
        }
        curr.next = newNode
    }
    this.size++
}

func (this *MyLinkedList) AddAtIndex(index int, val int) {
    if index > this.size {
        return
    }
    if index <= 0 {
        this.AddAtHead(val)
        return
    }
    newNode := &Node{val, nil}
    prev := this.head
    for i := 0; i < index-1; i++ {
        prev = prev.next
    }
    newNode.next = prev.next
    prev.next = newNode
    this.size++
}

func (this *MyLinkedList) DeleteAtIndex(index int) {
    if index < 0 || index >= this.size {
        return
    }
    if index == 0 {
        this.head = this.head.next
    } else {
        prev := this.head
        for i := 0; i < index-1; i++ {
            prev = prev.next
        }
        prev.next = prev.next.next
    }
    this.size--
}
```

### Complexity
- **Time Complexity**: 
  - `Get`: O(n) - we might need to traverse the list to find the desired node.
  - `AddAtHead`: O(1) - simply adjust the head pointer.
  - `AddAtTail`: O(n) - we might need to traverse to the end to add the node.
  - `AddAtIndex`: O(n) - potentially traverse to the position of insertion.
  - `DeleteAtIndex`: O(n) - might need to traverse to delete the node.
- **Space Complexity**: O(n) - each node takes up space in memory.

## Approach 3: Doubly Linked List Using Pointers

### Intuition
A doubly linked list, unlike a singly linked list, keeps a reference to both the next and previous nodes. This allows for bidirectional traversal, making certain operations more efficient, such as insertions and deletions at both ends and typically doubling the memory use per node to store the additional pointer.

### Implementation

```go
type DNode struct {
    val  int
    next *DNode
    prev *DNode
}

type MyLinkedList struct {
    head *DNode
    tail *DNode
    size int
}

func Constructor() MyLinkedList {
    return MyLinkedList{}
}

func (this *MyLinkedList) Get(index int) int {
    if index < 0 || index >= this.size {
        return -1
    }
    var curr *DNode
    if index < this.size/2 {
        curr = this.head
        for i := 0; i < index; i++ {
            curr = curr.next
        }
    } else {
        curr = this.tail
        for i := this.size - 1; i > index; i-- {
            curr = curr.prev
        }
    }
    return curr.val
}

func (this *MyLinkedList) AddAtHead(val int) {
    newNode := &DNode{val, this.head, nil}
    if this.size == 0 {
        this.tail = newNode
    } else {
        this.head.prev = newNode
    }
    this.head = newNode
    this.size++
}

func (this *MyLinkedList) AddAtTail(val int) {
    newNode := &DNode{val, nil, this.tail}
    if this.size == 0 {
        this.head = newNode
    } else {
        this.tail.next = newNode
    }
    this.tail = newNode
    this.size++
}

func (this *MyLinkedList) AddAtIndex(index int, val int) {
    if index > this.size {
        return
    }
    if index <= 0 {
        this.AddAtHead(val)
        return
    }
    if index == this.size {
        this.AddAtTail(val)
        return
    }

    curr := this.head
    for i := 0; i < index; i++ {
        curr = curr.next
    }
    newNode := &DNode{val, curr, curr.prev}
    curr.prev.next = newNode
    curr.prev = newNode
    this.size++
}

func (this *MyLinkedList) DeleteAtIndex(index int) {
    if index < 0 || index >= this.size {
        return
    }
    if index == 0 {
        this.head = this.head.next
        if this.head != nil {
            this.head.prev = nil
        } else {
            this.tail = nil
        }
    } else if index == this.size-1 {
        this.tail = this.tail.prev
        if this.tail != nil {
            this.tail.next = nil
        } else {
            this.head = nil
        }
    } else {
        curr := this.head
        for i := 0; i < index; i++ {
            curr = curr.next
        }
        curr.prev.next = curr.next
        curr.next.prev = curr.prev
    }
    this.size--
}
```

### Complexity
- **Time Complexity**: 
  - `Get`: O(n) - in the worst-case scenario.
  - `AddAtHead`: O(1) - simple head pointer adjustment.
  - `AddAtTail`: O(1) - simple tail pointer adjustment.
  - `AddAtIndex`: O(n) - potentially traverse to where we want to insert.
  - `DeleteAtIndex`: O(n) - might have to find the index to delete.
- **Space Complexity**: O(n) - linear space relative to the number of nodes in the list.

This problem is a great example of understanding how to manage nodes in memory and how pointers or references can be used to manage more complex data structures like a linked list. Each approach offers different trade-offs in operations and design, demonstrating the importance of choosing the right data structure for a problem's needs.

