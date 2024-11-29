# [707. Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Approach: Implementation of Singly Linked List

### Solution
```csharp
// Time Complexity: 
// - get: O(n)
// - addAtHead, addAtTail: O(1)
// - addAtIndex, deleteAtIndex: O(n)
// Space Complexity: O(n) (for storing nodes)

public class MyLinkedList {
    private class Node {
        public int Val;
        public Node Next;

        public Node(int val) {
            this.Val = val;
            this.Next = null;
        }
    }

    private Node head;
    private int size;

    public MyLinkedList() {
        head = null;
        size = 0;
    }

    public int Get(int index) {
        if (index < 0 || index >= size) {
            return -1;
        }

        Node current = head;
        for (int i = 0; i < index; i++) {
            current = current.Next;
        }
        return current.Val;
    }

    public void AddAtHead(int val) {
        Node newNode = new Node(val) {
            Next = head
        };
        head = newNode;
        size++;
    }

    public void AddAtTail(int val) {
        Node newNode = new Node(val);
        if (head == null) {
            head = newNode;
        } else {
            Node current = head;
            while (current.Next != null) {
                current = current.Next;
            }
            current.Next = newNode;
        }
        size++;
    }

    public void AddAtIndex(int index, int val) {
        if (index < 0 || index > size) {
            return;
        }
        if (index == 0) {
            AddAtHead(val);
        } else {
            Node newNode = new Node(val);
            Node current = head;
            for (int i = 0; i < index - 1; i++) {
                current = current.Next;
            }
            newNode.Next = current.Next;
            current.Next = newNode;
            size++;
        }
    }

    public void DeleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        if (index == 0) {
            head = head.Next;
        } else {
            Node current = head;
            for (int i = 0; i < index - 1; i++) {
                current = current.Next;
            }
            current.Next = current.Next.Next;
        }
        size--;
    }
}
```

