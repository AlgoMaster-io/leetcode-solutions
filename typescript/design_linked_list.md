# [707. Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Approach: Implementation of Singly Linked List

### Solution
typescript
```typescript
// Time Complexity: 
// - get: O(n)
// - addAtHead, addAtTail: O(1)
// - addAtIndex, deleteAtIndex: O(n)
// Space Complexity: O(n) (for storing nodes)

class MyLinkedList {
    private class Node {
        val: number;
        next: Node | null;

        constructor(val: number) {
            this.val = val;
            this.next = null;
        }
    }

    private head: Node | null;
    private size: number;

    constructor() {
        this.head = null;
        this.size = 0;
    }

    get(index: number): number {
        if (index < 0 || index >= this.size) {
            return -1;
        }

        let current = this.head;
        for (let i = 0; i < index; i++) {
            if (current) {
                current = current.next;
            }
        }
        return current ? current.val : -1;
    }

    addAtHead(val: number): void {
        const newNode = new this.Node(val);
        newNode.next = this.head;
        this.head = newNode;
        this.size++;
    }

    addAtTail(val: number): void {
        const newNode = new this.Node(val);
        if (!this.head) {
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

    addAtIndex(index: number, val: number): void {
        if (index < 0 || index > this.size) {
            return;
        }
        if (index === 0) {
            this.addAtHead(val);
        } else {
            const newNode = new this.Node(val);
            let current = this.head;
            for (let i = 0; i < index - 1; i++) {
                if (current) {
                    current = current.next;
                }
            }
            if (current) {
                newNode.next = current.next;
                current.next = newNode;
            }
            this.size++;
        }
    }

    deleteAtIndex(index: number): void {
        if (index < 0 || index >= this.size) {
            return;
        }
        if (index === 0) {
            this.head = this.head ? this.head.next : null;
        } else {
            let current = this.head;
            for (let i = 0; i < index - 1; i++) {
                if (current) {
                    current = current.next;
                }
            }
            if (current && current.next) {
                current.next = current.next.next;
            }
        }
        this.size--;
    }
}
```

