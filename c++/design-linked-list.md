# [Leetcode 707: Design Linked List](https://leetcode.com/problems/design-linked-list/)

## Approaches
- [Singly Linked List](#singly-linked-list)
- [Doubly Linked List](#doubly-linked-list)


## Singly Linked List

### Intuition
A Singly Linked List is a collection of nodes where each node holds a value and a reference to the next node in the list. This design is simple and works well for operations such as insertion or deletion if adjacent nodes are known. However, accessing a node by index requires O(n) traversal time.

### Operations
Let's design the operations one by one.

1. **Get**: To get the value at a particular index, traverse the list from the head node, and move step by step until you reach the desired index.
2. **Add at Head**: Insert a new node at the start of the list. Simply create a new node and make its next point to the current head before updating the head to this new node.
3. **Add at Tail**: Append a node at the end. Traverse until you reach the last node and then point its next to the new node.
4. **Add at Index**: To add a node at a specific index, traverse to that index and adjust the pointers accordingly to insert the node.
5. **Delete at Index**: Similar to insertion, traverse to the node just before the target, adjust its next to point to the node after the target.

### Code
```cpp
class MyLinkedList {
private:
    struct Node {
        int value;
        Node* next;
        Node(int val) : value(val), next(nullptr) {}
    };
    Node* head;
    int size;
    
public:
    /** Initialize your data structure here. */
    MyLinkedList() {
        head = nullptr;
        size = 0;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    int get(int index) {
        if (index < 0 || index >= size) return -1; // index out of bounds
        
        Node* current = head;
        for (int i = 0; i < index; i++) {
            current = current->next;
        }
        return current->value;
    }
    
    /** Add a node of value val before the first element of the linked list. */
    void addAtHead(int val) {
        Node* newNode = new Node(val);
        newNode->next = head;
        head = newNode;
        size++;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    void addAtTail(int val) {
        Node* newNode = new Node(val);
        if (!head) {
            head = newNode;
        } else {
            Node* current = head;
            while (current->next) {
                current = current->next;
            }
            current->next = newNode;
        }
        size++;
    }
    
    /** Add a node of value val before the index-th node in the linked list. */
    void addAtIndex(int index, int val) {
        if (index < 0 || index > size) return; // index out of bounds
        
        if (index == 0) {
            addAtHead(val);
            return;
        }
        
        Node* newNode = new Node(val);
        Node* current = head;
        for (int i = 0; i < index - 1; i++) {
            current = current->next;
        }
        newNode->next = current->next;
        current->next = newNode;
        size++;
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    void deleteAtIndex(int index) {
        if (index < 0 || index >= size) return; // index out of bounds
        
        Node* toDelete;
        if (index == 0) {
            toDelete = head;
            head = head->next;
        } else {
            Node* current = head;
            for (int i = 0; i < index - 1; i++) {
                current = current->next;
            }
            toDelete = current->next;
            current->next = current->next->next;
        }
        delete toDelete;
        size--;
    }
};
```
### Complexity
- **Time Complexity**: O(n) for get, addAtIndex, deleteAtIndex due to traversal.
- **Space Complexity**: O(1) for each operation as no extra space apart from variables is used.

## Doubly Linked List

### Intuition
A Doubly Linked List extends the functionality of a singly linked list by allowing bidirectional traversal. Each node has two pointers: one to the next node and another to the previous node. This allows more efficient implementation for some operations since we can traverse the list in both directions.

### Operations
Similar operations as the singly linked list but account for adjusting both previous and next pointers:

1. **Get**: Same traversal from head as singly linked list.
2. **Add at Head**: Update both the new node's next and current head's prev.
3. **Add at Tail**: Traverse to tail and update both pointers.
4. **Add at Index**: Adjust both prev and next pointers when inserting a new node.
5. **Delete at Index**: Similarly, update prev and next of adjacent nodes.

### Code
```cpp
class MyLinkedList {
private:
    struct Node {
        int value;
        Node* next;
        Node* prev;
        Node(int val) : value(val), next(nullptr), prev(nullptr) {}
    };
    Node* head;
    Node* tail;
    int size;
    
public:
    /** Initialize your data structure here. */
    MyLinkedList() {
        head = nullptr;
        tail = nullptr;
        size = 0;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    int get(int index) {
        if (index < 0 || index >= size) return -1; // index out of bounds
        
        Node* current = head;
        for (int i = 0; i < index; i++) {
            current = current->next;
        }
        return current->value;
    }
    
    /** Add a node of value val before the first element of the linked list. */
    void addAtHead(int val) {
        Node* newNode = new Node(val);
        newNode->next = head;
        if (head) {
            head->prev = newNode;
        }
        head = newNode;
        if (size == 0) {
            tail = newNode; // if list was empty
        }
        size++;
    }
    
    /** Append a node of value val to the last element of the linked list. */
    void addAtTail(int val) {
        Node* newNode = new Node(val);
        if (!head) {
            head = newNode;
            tail = newNode;
        } else {
            tail->next = newNode;
            newNode->prev = tail;
            tail = newNode;
        }
        size++;
    }
    
    /** Add a node of value val before the index-th node in the linked list. */
    void addAtIndex(int index, int val) {
        if (index < 0 || index > size) return; // index out of bounds
        
        if (index == 0) {
            addAtHead(val);
            return;
        }
        
        if (index == size) {
            addAtTail(val);
            return;
        }
        
        Node* newNode = new Node(val);
        Node* current = head;
        for (int i = 0; i < index; i++) {
            current = current->next;
        }
        newNode->next = current;
        newNode->prev = current->prev;
        current->prev->next = newNode;
        current->prev = newNode;
        
        size++;
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    void deleteAtIndex(int index) {
        if (index < 0 || index >= size) return; // index out of bounds
        
        Node* toDelete;
        if (index == 0) {
            toDelete = head;
            head = head->next;
            if (head) head->prev = nullptr;
            if (size == 1) tail = nullptr; // if it was the only node
        } else if (index == size - 1) {
            toDelete = tail;
            tail = tail->prev;
            if (tail) tail->next = nullptr;
        } else {
            Node* current = head;
            for (int i = 0; i < index; i++) {
                current = current->next;
            }
            toDelete = current;
            current->prev->next = current->next;
            current->next->prev = current->prev;
        }
        delete toDelete;
        size--;
    }
};
```
### Complexity
- **Time Complexity**: O(n) for get, addAtIndex, deleteAtIndex due to traversal.
- **Space Complexity**: O(1) for each operation, though there's a minor overhead due to additional pointers.

