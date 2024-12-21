# [Leetcode 707: Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Solutions
- [Approach 1: Singly Linked List](#approach-1-singly-linked-list)
- [Approach 2: Doubly Linked List](#approach-2-doubly-linked-list)
- [Approach 3: Implement with Tail Reference](#approach-3-implement-with-tail-reference)

### Approach 1: Singly Linked List

#### Intuition
A singly linked list is a basic data structure where each node points to the next node, but there's no reference to the previous node. This structure allows for straightforward insertion and deletion operations at the head. We will maintain a head pointer to the start of the list and a length variable to keep track of the number of elements.

#### Implementation

```csharp
public class MyLinkedList {

    private class Node {
        public int val;
        public Node next;
        
        public Node(int val = 0, Node next = null) {
            this.val = val;
            this.next = next;
        }
    }
    
    private Node head;
    private int length;
    
    public MyLinkedList() {
        head = null;
        length = 0;
    }
    
    public int Get(int index) {
        if (index < 0 || index >= length) return -1;
        Node current = head;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.val;
    }
    
    public void AddAtHead(int val) {
        Node newNode = new Node(val, head);
        head = newNode;
        length++;
    }
    
    public void AddAtTail(int val) {
        Node newNode = new Node(val);
        if (head == null) {
            head = newNode;
        } else {
            Node current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = newNode;
        }
        length++;
    }
    
    public void AddAtIndex(int index, int val) {
        if (index < 0 || index > length) return;
        if (index == 0) {
            AddAtHead(val);
        } else {
            Node newNode = new Node(val);
            Node current = head;
            for (int i = 0; i < index - 1; i++) {
                current = current.next;
            }
            newNode.next = current.next;
            current.next = newNode;
            length++;
        }
    }
    
    public void DeleteAtIndex(int index) {
        if (index < 0 || index >= length) return;
        if (index == 0) {
            head = head.next;
        } else {
            Node current = head;
            for (int i = 0; i < index - 1; i++) {
                current = current.next;
            }
            current.next = current.next.next;
        }
        length--;
    }
}
```

**Time Complexity**:  
- `Get`, `AddAtTail`: O(n) where n is the number of nodes in the linked list.
- `AddAtHead`, `DeleteAtIndex`, `AddAtIndex`: O(n) since in the worst case, we need to reach up to the node just before the immediate location of interest.

**Space Complexity**: O(1) as we're using constant space.

### Approach 2: Doubly Linked List

#### Intuition
By using a doubly linked list, each node has a reference to both the next and the previous nodes. This enables certain operations (like delete) to be more efficient by skipping backtracking.

#### Implementation

```csharp
public class MyLinkedList {

    private class Node {
        public int val;
        public Node next;
        public Node prev;
        
        public Node(int val = 0, Node next = null, Node prev = null) {
            this.val = val;
            this.next = next;
            this.prev = prev;
        }
    }
    
    private Node head;
    private Node tail;
    private int length;
    
    public MyLinkedList() {
        head = new Node(); // Sentinel head
        tail = new Node(); // Sentinel tail
        head.next = tail;
        tail.prev = head;
        length = 0;
    }
    
    public int Get(int index) {
        if (index < 0 || index >= length) return -1;
        Node current = head.next;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.val;
    }
    
    public void AddAtHead(int val) {
        AddAtIndex(0, val);
    }
    
    public void AddAtTail(int val) {
        AddAtIndex(length, val);
    }
    
    public void AddAtIndex(int index, int val) {
        if (index < 0 || index > length) return;
        Node prev = head;
        for (int i = 0; i < index; i++) {
            prev = prev.next;
        }
        Node next = prev.next;
        Node newNode = new Node(val, next, prev);
        prev.next = newNode;
        next.prev = newNode;
        length++;
    }
    
    public void DeleteAtIndex(int index) {
        if (index < 0 || index >= length) return;
        Node prev = head;
        for (int i = 0; i < index; i++) {
            prev = prev.next;
        }
        Node toDelete = prev.next;
        Node next = toDelete.next;
        prev.next = next;
        next.prev = prev;
        length--;
    }
}
```

**Time Complexity**:  
- `Get`, `AddAtIndex`, `DeleteAtIndex`: O(min(index, n - index)) due to bidirectional traversal from either the head or tail.
- `AddAtHead`, `AddAtTail`: O(1).

**Space Complexity**: O(1).

### Approach 3: Implement with Tail Reference

#### Intuition
We can enhance the basic singly linked list by maintaining a reference to the tail node. This makes `AddAtTail` more efficient by avoiding traversal.

#### Implementation

```csharp
public class MyLinkedList {

    private class Node {
        public int val;
        public Node next;
        
        public Node(int val = 0, Node next = null) {
            this.val = val;
            this.next = next;
        }
    }
    
    private Node head;
    private Node tail;
    private int length;
    
    public MyLinkedList() {
        head = null;
        tail = null;
        length = 0;
    }
    
    public int Get(int index) {
        if (index < 0 || index >= length) return -1;
        Node current = head;
        for (int i = 0; i < index; i++) {
            current = current.next;
        }
        return current.val;
    }
    
    public void AddAtHead(int val) {
        Node newNode = new Node(val, head);
        head = newNode;
        if (tail == null) {
            tail = head;
        }
        length++;
    }
    
    public void AddAtTail(int val) {
        Node newNode = new Node(val);
        if (tail != null) {
            tail.next = newNode;
        }
        tail = newNode;
        if (head == null) {
            head = tail;
        }
        length++;
    }
    
    public void AddAtIndex(int index, int val) {
        if (index < 0 || index > length) return;
        if (index == 0) {
            AddAtHead(val);
        } else if (index == length) {
            AddAtTail(val);
        } else {
            Node newNode = new Node(val);
            Node current = head;
            for (int i = 0; i < index - 1; i++) {
                current = current.next;
            }
            newNode.next = current.next;
            current.next = newNode;
            length++;
        }
    }
    
    public void DeleteAtIndex(int index) {
        if (index < 0 || index >= length) return;
        if (index == 0) {
            head = head.next;
            if (head == null) {
                tail = null;
            }
        } else {
            Node current = head;
            for (int i = 0; i < index - 1; i++) {
                current = current.next;
            }
            current.next = current.next.next;
            if (current.next == null) {
                tail = current;
            }
        }
        length--;
    }
}
```

**Time Complexity**:  
- `Get`: O(n)
- `AddAtHead`, `AddAtTail`: O(1)
- `AddAtIndex`, `DeleteAtIndex`: O(index) due to the linked list traversal.

**Space Complexity**: O(1).

