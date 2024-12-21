# [Leetcode 707: Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Approaches:
1. [Basic Approach: Using Array](#basic-approach-using-array)
2. [Intermediate Approach: Singly Linked List](#intermediate-approach-singly-linked-list)
3. [Optimal Approach: Doubly Linked List](#optimal-approach-doubly-linked-list)

## Basic Approach: Using Array

### Intuition:
The simplest way to implement a linked list is to use an array to store values. This approach serves as an introductory concept of how to manage linked list operations using available array functions like `push`, `splice`, etc. While this violates the principle of linked lists, it helps to grasp basic list operations.

### Code:
```javascript
class MyLinkedList {
    constructor() {
        this.list = [];
    }

    get(index) {
        // If the index is out of bounds
        if (index < 0 || index >= this.list.length) return -1;
        return this.list[index];  // Return the value at the index
    }

    addAtHead(val) {
        // Add the value at the start of the list
        this.list.unshift(val);
    }

    addAtTail(val) {
        // Add the value at the end of the list
        this.list.push(val);
    }

    addAtIndex(index, val) {
        // If index is within bounds, insert the value
        if (index < 0 || index > this.list.length) return;
        this.list.splice(index, 0, val);
    }

    deleteAtIndex(index) {
        // If index is within bounds, delete the element
        if (index < 0 || index >= this.list.length) return;
        this.list.splice(index, 1);
    }
}
```

### Complexity:
- Time: `O(n)` for add and delete operations due to element shifting.
- Space: `O(n)`

---

## Intermediate Approach: Singly Linked List

### Intuition:
A singly linked list consists of nodes where each node points to the next node in the sequence. It allows easier deletion and insertion as we only need to update the pointers. This is more efficient than shifting elements as in arrays.

### Code:
```javascript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

class MyLinkedList {
    constructor() {
        this.head = null;
        this.size = 0;
    }

    get(index) {
        // If the index is out of bounds
        if (index < 0 || index >= this.size) return -1;
        let current = this.head;
        for (let i = 0; i < index; i++) {
            current = current.next;  // Traverse the list to reach the index
        }
        return current.val;  // Return the value at the index
    }

    addAtHead(val) {
        const newNode = new ListNode(val, this.head);
        this.head = newNode;  // Point the head to the new node
        this.size++;
    }

    addAtTail(val) {
        const newNode = new ListNode(val);
        if (!this.head) {
            this.head = newNode;  // If list is empty, new node is the head
        } else {
            let current = this.head;
            while (current.next) {
                current = current.next;  // Traverse to the end of the list
            }
            current.next = newNode;  // Append the new node
        }
        this.size++;
    }

    addAtIndex(index, val) {
        if (index > this.size || index < 0) return;  // Index out of valid range
        if (index === 0) {
            this.addAtHead(val);
            return;
        }
        let current = this.head;
        for (let i = 0; i < index - 1; i++) {
            current = current.next;  // Traverse to the node before index
        }
        const newNode = new ListNode(val, current.next);
        current.next = newNode;  // Insert the new node
        this.size++;
    }

    deleteAtIndex(index) {
        if (index < 0 || index >= this.size) return;  // Index out of valid range
        if (index === 0) {
            this.head = this.head.next;  // Remove the head node
        } else {
            let current = this.head;
            for (let i = 0; i < index - 1; i++) {
                current = current.next;  // Traverse to the node before index
            }
            current.next = current.next.next;  // Remove the node at index
        }
        this.size--;
    }
}
```

### Complexity:
- Time: `O(n)` for most operations (traversal required to get to index)
- Space: `O(n)`

---

## Optimal Approach: Doubly Linked List

### Intuition:
Doubly linked lists enhance the singly linked list by having a pointer to both the previous and next nodes. This allows for efficient forward and backward traversal and easier deletion operations since each node has a reference to its predecessor.

### Code:
```javascript
class ListNode {
    constructor(val = 0, next = null, prev = null) {
        this.val = val;
        this.next = next;
        this.prev = prev;
    }
}

class MyLinkedList {
    constructor() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    get(index) {
        if (index < 0 || index >= this.size) return -1;
        let current;
        // Traverse from the head if index is in the first half
        if (index < this.size / 2) {
            current = this.head;
            for (let i = 0; i < index; i++) {
                current = current.next;
            }
        } else {
            // Traverse from the tail if index is in the second half
            current = this.tail;
            for (let i = this.size - 1; i > index; i--) {
                current = current.prev;
            }
        }
        return current.val;
    }

    addAtHead(val) {
        const newNode = new ListNode(val, this.head);
        if (this.head) this.head.prev = newNode;
        this.head = newNode;
        // If list was empty, both head and tail should point to new node
        if (!this.tail) this.tail = newNode;
        this.size++;
    }

    addAtTail(val) {
        const newNode = new ListNode(val, null, this.tail);
        if (this.tail) this.tail.next = newNode;
        this.tail = newNode;
        // If list was empty, both head and tail should point to new node
        if (!this.head) this.head = newNode;
        this.size++;
    }

    addAtIndex(index, val) {
        if (index > this.size || index < 0) return;
        if (index === 0) {
            this.addAtHead(val);
            return;
        }
        if (index === this.size) {
            this.addAtTail(val);
            return;
        }
        let current;
        // Use similar traversal optimization
        if (index < this.size / 2) {
            current = this.head;
            for (let i = 0; i < index; i++) {
                current = current.next;
            }
        } else {
            current = this.tail;
            for (let i = this.size - 1; i >= index; i--) {
                current = current.prev;
            }
        }
        const newNode = new ListNode(val, current, current.prev);
        current.prev.next = newNode;
        current.prev = newNode;
        this.size++;
    }

    deleteAtIndex(index) {
        if (index < 0 || index >= this.size) return;
        let current;
        if (index < this.size / 2) {
            current = this.head;
            for (let i = 0; i < index; i++) {
                current = current.next;
            }
        } else {
            current = this.tail;
            for (let i = this.size - 1; i > index; i--) {
                current = current.prev;
            }
        }
        if (current.prev) current.prev.next = current.next;
        else this.head = current.next;
        if (current.next) current.next.prev = current.prev;
        else this.tail = current.prev;
        this.size--;
    }
}
```

### Complexity:
- Time: `O(n)` for most operations; enhanced traversal efficiency due to bidirectional traversal.
- Space: `O(n)` 

In conclusion, we see that while the doubly linked list offers more efficient operations with fewer limitations in traversal, it comes with increased memory usage and complexity in code compared to singly linked lists.

