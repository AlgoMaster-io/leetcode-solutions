# [Leetcode 707: Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Approaches:
1. [Singly Linked List](#singly-linked-list)
2. [Doubly Linked List](#doubly-linked-list)

---

### Singly Linked List

#### Intuition
A singly linked list is a data structure where each element (node) contains a value and a reference to the next node in the list. This structure allows sequential access but only in one direction (from head to tail).

Operations such as addition or deletion at the head, or traversing to a specific element, are straightforward. However, performing operations at the tail or arbitrary indices requires traversal from the head, resulting in linear time complexities for some operations.

#### Steps:
- Define a class `Node` with a value and a next reference.
- Implement a `LinkedList` class with methods for adding, removing, and getting values from specified positions.
- Maintain a size property to keep track of the number of elements.

```typescript
class ListNode {
    val: number;
    next: ListNode | null;

    constructor(val: number, next: ListNode | null = null) {
        this.val = val;
        this.next = next;
    }
}

class MyLinkedList {
    private head: ListNode | null;
    private size: number;

    constructor() {
        this.head = null;
        this.size = 0;
    }

    get(index: number): number {
        if (index < 0 || index >= this.size) return -1;

        let current = this.head;
        for (let i = 0; i < index; i++) {
            if (current) current = current.next;
        }
        return current ? current.val : -1;
    }

    addAtHead(val: number): void {
        const newNode = new ListNode(val, this.head);
        this.head = newNode;
        this.size++;
    }

    addAtTail(val: number): void {
        const newNode = new ListNode(val);
        if (!this.head) {
            this.head = newNode;
        } else {
            let current = this.head;
            while (current.next) {
                current = current.next;
            }
            current.next = newNode;
        }
        this.size++;
    }

    addAtIndex(index: number, val: number): void {
        if (index < 0 || index > this.size) return;

        if (index === 0) {
            this.addAtHead(val);
        } else {
            let current = this.head;
            for (let i = 0; i < index - 1; i++) {
                if (current) current = current.next;
            }

            const newNode = new ListNode(val, current!.next);
            current!.next = newNode;
            this.size++;
        }
    }

    deleteAtIndex(index: number): void {
        if (index < 0 || index >= this.size) return;

        if (index === 0) {
            this.head = this.head?.next || null;
        } else {
            let current = this.head;
            for (let i = 0; i < index - 1; i++) {
                if (current) current = current.next;
            }
            if (current && current.next) {
                current.next = current.next.next;
            }
        }
        this.size--;
    }
}
```

**Time Complexity:**
- `get`: O(n)
- `addAtHead`: O(1)
- `addAtTail`: O(n)
- `addAtIndex`: O(n)
- `deleteAtIndex`: O(n)

**Space Complexity:** O(n) for storing the list

---

### Doubly Linked List

#### Intuition
A doubly linked list enhances the singly linked list by adding a previous reference to each node. This allows traversal in both directions and improves efficiency for operations at the tail, as we can maintain a reference to the last node.

Traversal and insertion operations become more efficient compared to singly linked lists, as we can perform operations at both ends of the list in constant time.

#### Steps:
- Define a class `DoublyListNode` with value, next, and prev references.
- Implement a `DoublyLinkedList` class, maintaining head and tail references for efficient access.

```typescript
class DoublyListNode {
    val: number;
    next: DoublyListNode | null;
    prev: DoublyListNode | null;

    constructor(val: number, next: DoublyListNode | null = null, prev: DoublyListNode | null = null) {
        this.val = val;
        this.next = next;
        this.prev = prev;
    }
}

class MyDoublyLinkedList {
    private head: DoublyListNode | null;
    private tail: DoublyListNode | null;
    private size: number;

    constructor() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    get(index: number): number {
        if (index < 0 || index >= this.size) return -1;

        let current: DoublyListNode | null = this.head;
        if (index < this.size / 2) {
            for (let i = 0; i < index; i++) {
                if (current) current = current!.next;
            }
        } else {
            current = this.tail;
            for (let i = this.size - 1; i > index; i--) {
                if (current) current = current!.prev;
            }
        }

        return current ? current.val : -1;
    }

    addAtHead(val: number): void {
        let newNode = new DoublyListNode(val, this.head, null);
        if (this.head) this.head.prev = newNode;
        this.head = newNode;
        if (!this.tail) this.tail = newNode;
        this.size++;
    }

    addAtTail(val: number): void {
        let newNode = new DoublyListNode(val, null, this.tail);
        if (this.tail) this.tail.next = newNode;
        this.tail = newNode;
        if (!this.head) this.head = newNode;
        this.size++;
    }

    addAtIndex(index: number, val: number): void {
        if (index < 0 || index > this.size) return;

        if (index === 0) {
            this.addAtHead(val);
        } else if (index === this.size) {
            this.addAtTail(val);
        } else {
            let current: DoublyListNode | null = this.head;
            if (index < this.size / 2) {
                for (let i = 0; i < index - 1; i++) {
                    if (current) current = current.next;
                }
            } else {
                current = this.tail;
                for (let i = this.size - 1; i > index - 1; i--) {
                    if (current) current = current.prev;
                }
            }

            const newNode = new DoublyListNode(val, current!.next, current);
            if (current && current.next) current.next.prev = newNode;
            if (current) current.next = newNode;
            this.size++;
        }
    }

    deleteAtIndex(index: number): void {
        if (index < 0 || index >= this.size) return;

        if (index === 0) {
            if (this.head) {
                this.head = this.head.next;
                if (this.head) this.head.prev = null;
            }
        } else if (index === this.size - 1) {
            if (this.tail) {
                this.tail = this.tail.prev;
                if (this.tail) this.tail.next = null;
            }
        } else {
            let current: DoublyListNode | null = this.head;
            if (index < this.size / 2) {
                for (let i = 0; i < index - 1; i++) {
                    if (current) current = current.next;
                }
            } else {
                current = this.tail;
                for (let i = this.size - 1; i > index - 1; i--) {
                    if (current) current = current.prev;
                }
            }

            if (current && current.next && current.next.next) {
                current.next = current.next.next;
                current.next.prev = current;
            }
        }
        this.size--;
    }
}
```

**Time Complexity**: 
- `get`: O(n)
- `addAtHead`: O(1)
- `addAtTail`: O(1)
- `addAtIndex`: O(n)
- `deleteAtIndex`: O(n)

**Space Complexity:** O(n) for storing the list

This doubly linked list is more efficient for certain operations, especially those involving the tail, as it provides direct access without requiring traversal from the head.

