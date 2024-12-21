# [Leetcode 707: Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Approaches
1. [Single Linked List Implementation](#single-linked-list-implementation)
2. [Doubly Linked List Implementation](#doubly-linked-list-implementation)

### Single Linked List Implementation

**Intuition:**

A singly linked list consists of nodes where each node contains a data field and a reference(link) to the next node in the sequence. To solve this problem, we will incrementally build a singly linked list with basic operations such as `add`, `delete`, and `get` by manipulating these links properly.

**Implementation Steps:**

1. **Node Class**: We'll define a simple class for node construction.
2. **LinkedList Class**: 
    - **Initialization**: We maintain a `head` pointer to the first element and a `size` variable to track the number of elements.
    - **Get**: To get an element at a specific index, traverse from the head node, iterating through links until the desired index is reached.
    - **AddAtHead**: Insert a new node at the head.
    - **AddAtTail**: Traverse to the end of the list and insert the new node.
    - **AddAtIndex**: For a specific index, traverse to the node just before the target index and adjust pointers to include the new node.
    - **DeleteAtIndex**: Similar to AddAtIndex, traverse to the node just before the target, then adjust pointers to remove the target node.

**Code:**
```python
class Node:
    def __init__(self, value=0, next=None):
        self.value = value
        self.next = next

class MyLinkedList:

    def __init__(self):
        self.head = None
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        current = self.head
        for _ in range(index):
            current = current.next
        return current.value

    def addAtHead(self, val: int) -> None:
        newNode = Node(val, self.head)
        self.head = newNode
        self.size += 1

    def addAtTail(self, val: int) -> None:
        newNode = Node(val)
        if self.size == 0:
            self.head = newNode
        else:
            current = self.head
            while current.next:
                current = current.next
            current.next = newNode
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
        if index == 0:
            return self.addAtHead(val)
        newNode = Node(val)
        current = self.head
        for _ in range(index - 1):
            current = current.next
        newNode.next = current.next
        current.next = newNode
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        if index == 0:
            self.head = self.head.next
        else:
            current = self.head
            for _ in range(index - 1):
                current = current.next
            current.next = current.next.next
        self.size -= 1
```
**Time Complexity:**
- `get`, `addAtTail`, `addAtIndex`, `deleteAtIndex`: \(O(n)\), where \(n\) is the number of nodes.
- `addAtHead`: \(O(1)\).

**Space Complexity:**
- \(O(1)\) for each operation, excluding storage for the actual linked list.

### Doubly Linked List Implementation

**Intuition:**

A doubly linked list allows backward traversal due to nodes having a reference to both the next and previous nodes, facilitating certain operations more efficiently, especially deletions and insertions near the end.

**Implementation Steps:**

1. **Node Class**: As before, but with added `prev` reference.
2. **LinkedList Class**:
    - Keeps `head` and `tail` pointers for direct access.
    - Operations are similar in logic as the singly linked list, but require careful attention to handling the `prev` pointer.

**Code:**
```python
class Node:
    def __init__(self, value=0, next=None, prev=None):
        self.value = value
        self.next = next
        self.prev = prev

class MyLinkedList:

    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        current = self.head if index < self.size // 2 else self.tail
        if index < self.size // 2:
            for _ in range(index):
                current = current.next
        else:
            for _ in range(self.size - index - 1):
                current = current.prev
        return current.value

    def addAtHead(self, val: int) -> None:
        newNode = Node(val, next=self.head)
        if self.head:
            self.head.prev = newNode
        self.head = newNode
        if self.size == 0:
            self.tail = newNode
        self.size += 1

    def addAtTail(self, val: int) -> None:
        newNode = Node(val, prev=self.tail)
        if self.tail:
            self.tail.next = newNode
        self.tail = newNode
        if self.size == 0:
            self.head = newNode
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
        if index == 0:
            self.addAtHead(val)
            return
        if index == self.size:
            self.addAtTail(val)
            return
        current = self.head if index < self.size // 2 else self.tail
        if index < self.size // 2:
            for _ in range(index - 1):
                current = current.next
        else:
            for _ in range(self.size - index):
                current = current.prev
        newNode = Node(val, current.next, current)
        current.next.prev = newNode
        current.next = newNode
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        if index == 0:
            if self.head.next:
                self.head = self.head.next
                self.head.prev = None
            else:
                self.head = None
                self.tail = None
        elif index == self.size - 1:
            self.tail = self.tail.prev
            if self.tail:
                self.tail.next = None
            else:
                self.head = None
        else:
            current = self.head if index < self.size // 2 else self.tail
            if index < self.size // 2:
                for _ in range(index):
                    current = current.next
            else:
                for _ in range(self.size - index - 1):
                    current = current.prev
            current.prev.next = current.next
            current.next.prev = current.prev
        self.size -= 1
```
**Time Complexity:**
- `get`, `addAtTail`, `addAtIndex`, `deleteAtIndex`: \(O(n/2)\) on average, but still \(O(n)\) in worst-case scenario.
- `addAtHead`: \(O(1)\).

**Space Complexity:**
- \(O(1)\) for each operation, excluding storage for the actual linked list.

Implementing a doubly linked list provides more flexibility and can make certain operations more efficient, particularly for operations near the end of the list.

