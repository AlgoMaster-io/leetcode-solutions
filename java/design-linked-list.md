# [Leetcode 707: Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Approaches
1. [Singly Linked List](#singly-linked-list)
2. [Doubly Linked List](#doubly-linked-list)

## Singly Linked List

### Intuition
A singly linked list is a data structure that consists of a sequence of elements, where each element points to the next one. The basic operations include adding an element, removing an element, and retrieving an element at a specific index. The singly linked list is efficient for insertion and deletion operations as it doesn't require shifting elements like an array does. The challenge is to efficiently handle edge cases such as indexing beyond the bounds of the list.

### Approach
For a singly linked list:
- Maintain a `ListNode` class with properties for the value and a pointer to the next node.
- Keep track of the head of the list and the size to manage insertion and deletion at specific points.
- The add operation involves traversing to the desired index and adjusting the pointers.
- The delete operation involves traversing and re-linking the list to skip the deleted node.

```java
class MyLinkedList {
    private class ListNode {
        int val;
        ListNode next;
        
        ListNode(int val) {
            this.val = val;
        }
    }
    
    private ListNode head;
    private int size;

    public MyLinkedList() {
        this.head = null;
        this.size = 0;
    }
    
    public int get(int index) {
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode current = head;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.val;
    }
    
    public void addAtHead(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = head;
        head = newNode;
        size++;
    }
    
    public void addAtTail(int val) {
        ListNode newNode = new ListNode(val);
        if (head == null) {
            head = newNode;
        } else {
            ListNode current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = newNode;
        }
        size++;
    }
    
    public void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        if (index <= 0) {
            addAtHead(val);
        } else {
            ListNode newNode = new ListNode(val);
            ListNode current = head;
            for (int i = 0; i < index - 1; i++) {
                current = current.next;
            }
            newNode.next = current.next;
            current.next = newNode;
            size++;
        }
    }
    
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        if (index == 0) {
            head = head.next;
        } else {
            ListNode current = head;
            for (int i = 0; i < index - 1; i++) {
                current = current.next;
            }
            current.next = current.next.next;
        }
        size--;
    }
}
```

### Complexity Analysis
- **Time Complexity**: `O(n)` for get, add, and delete operations as each requires traversal.
- **Space Complexity**: `O(1)` for each operation as we maintain a fixed number of references regardless of input size.

## Doubly Linked List

### Intuition
The doubly linked list extends the singly linked list by adding a previous pointer to each node. This allows for efficient bidirectional traversal. It helps improve the deletion operation by providing direct access to the previous node, thereby avoiding full traversal for backtracking operations.

### Approach
For a doubly linked list:
- Extend the node definition to include pointers to both previous and next nodes.
- Maintain both head and tail pointers to easily add elements to both ends.
- Update operations (add, delete) ensure that previous pointers are maintained along with next pointers.

```java
class MyDoublyLinkedList {
    private class ListNode {
        int val;
        ListNode prev, next;
        
        ListNode(int val) {
            this.val = val;
        }
    }
    
    private ListNode head, tail;
    private int size;

    public MyDoublyLinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
    
    public int get(int index) {
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode current;
        if (index < size / 2) {
            current = head;
            for (int i = 0; i < index; i++) {
                current = current.next;
            }
        } else {
            current = tail;
            for (int i = size - 1; i > index; i--) {
                current = current.prev;
            }
        }
        return current.val;
    }
    
    public void addAtHead(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = head;
        if (head != null) {
            head.prev = newNode;
        } else {
            tail = newNode;
        }
        head = newNode;
        size++;
    }
    
    public void addAtTail(int val) {
        ListNode newNode = new ListNode(val);
        newNode.prev = tail;
        if (tail != null) {
            tail.next = newNode;
        } else {
            head = newNode;
        }
        tail = newNode;
        size++;
    }
    
    public void addAtIndex(int index, int val) {
        if (index > size) return;
        if (index <= 0) {
            addAtHead(val);
        } else if (index == size) {
            addAtTail(val);
        } else {
            ListNode newNode = new ListNode(val);
            ListNode current;
            if (index < size / 2) {
                current = head;
                for (int i = 0; i < index - 1; i++) {
                    current = current.next;
                }
            } else {
                current = tail;
                for (int i = size; i > index; i--) {
                    current = current.prev;
                }
            }
            newNode.next = current.next;
            current.next.prev = newNode;
            current.next = newNode;
            newNode.prev = current;
            size++;
        }
    }
    
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) return;
        if (index == 0) {
            if (head.next != null) {
                head = head.next;
                head.prev = null;
            } else {
                head = tail = null;
            }
        } else if (index == size - 1) {
            if (tail.prev != null) {
                tail = tail.prev;
                tail.next = null;
            } else {
                head = tail = null;
            }
        } else {
            ListNode current;
            if (index < size / 2) {
                current = head;
                for (int i = 0; i < index; i++) {
                    current = current.next;
                }
            } else {
                current = tail;
                for (int i = size - 1; i > index; i--) {
                    current = current.prev;
                }
            }
            current.prev.next = current.next;
            current.next.prev = current.prev;
        }
        size--;
    }
}
```

### Complexity Analysis
- **Time Complexity**: `O(n)` for get, add, and delete operations. The selection between forward or backward traversal provides a slight optimization.
- **Space Complexity**: `O(1)` for each operation; each node holds additional reference, but overall the space is linear with the number of elements.

By implementing and understanding these two approaches, you get deeper insights into the core concepts of linked lists and their operations.

