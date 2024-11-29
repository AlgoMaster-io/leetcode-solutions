# [707. Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Approach: Implementation of Singly Linked List

### Solution

```javascript
// Time Complexity: 
// - get: O(n)
// - addAtHead, addAtTail: O(1)
// - addAtIndex, deleteAtIndex: O(n)
// Space Complexity: O(n) (for storing nodes)

class MyLinkedList {
    constructor() {
        this.head = null;
        this.size = 0;
    }
    
    // Node class to create a new node
    class Node {
        constructor(val) {
            this.val = val;
            this.next = null;
        }
    }

    get(index) {
        if (index < 0 || index >= this.size) {
            return -1;
        }

        let current = this.head;
        for (let i = 0; i < index; i++) {
            current = current.next;
        }
        return current.val;
    }

    addAtHead(val) {
        const newNode = new Node(val);
        newNode.next = this.head;
        this.head = newNode;
        this.size++;
    }

    addAtTail(val) {
        const newNode = new Node(val);
        if (this.head === null) {
            this.head = newNode;
        } else {
            let current = this.head;
            while (current.next !== null) {
                current = current.next;
            }
            current.next = newNode;
        }
        this.size++;
    }

    addAtIndex(index, val) {
        if (index < 0 || index > this.size) {
            return;
        }
        if (index === 0) {
            this.addAtHead(val);
        } else {
            const newNode = new Node(val);
            let current = this.head;
            for (let i = 0; i < index - 1; i++) {
                current = current.next;
            }
            newNode.next = current.next;
            current.next = newNode;
            this.size++;
        }
    }

    deleteAtIndex(index) {
        if (index < 0 || index >= this.size) {
            return;
        }
        if (index === 0) {
            this.head = this.head.next;
        } else {
            let current = this.head;
            for (let i = 0; i < index - 1; i++) {
                current = current.next;
            }
            current.next = current.next.next;
        }
        this.size--;
    }
}
```

