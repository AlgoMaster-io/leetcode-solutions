# [707. Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Approach: Implementation of Singly Linked List

### Solution
go
```go
// Time Complexity: 
// - Get: O(n)
// - AddAtHead, AddAtTail: O(1)
// - AddAtIndex, DeleteAtIndex: O(n)
// Space Complexity: O(n) (for storing nodes)

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

    current := this.head
    for i := 0; i < index; i++ {
        current = current.next
    }
    return current.val
}

func (this *MyLinkedList) AddAtHead(val int) {
    newNode := &Node{val: val}
    newNode.next = this.head
    this.head = newNode
    this.size++
}

func (this *MyLinkedList) AddAtTail(val int) {
    newNode := &Node{val: val}
    if this.head == nil {
        this.head = newNode
    } else {
        current := this.head
        for current.next != nil {
            current = current.next
        }
        current.next = newNode
    }
    this.size++
}

func (this *MyLinkedList) AddAtIndex(index int, val int) {
    if index < 0 || index > this.size {
        return
    }
    if index == 0 {
        this.AddAtHead(val)
    } else {
        newNode := &Node{val: val}
        current := this.head
        for i := 0; i < index-1; i++ {
            current = current.next
        }
        newNode.next = current.next
        current.next = newNode
        this.size++
    }
}

func (this *MyLinkedList) DeleteAtIndex(index int) {
    if index < 0 || index >= this.size {
        return
    }
    if index == 0 {
        this.head = this.head.next
    } else {
        current := this.head
        for i := 0; i < index-1; i++ {
            current = current.next
        }
        current.next = current.next.next
    }
    this.size--
}
```


