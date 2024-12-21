# [LRU Cache - LeetCode](https://leetcode.com/problems/lru-cache/)

## Approaches
- [Approach 1: Doubly Linked List with HashMap (Basic)](#approach-1-doubly-linked-list-with-hashmap-basic)
- [Approach 2: Doubly Linked List with HashMap (Optimized)](#approach-2-doubly-linked-list-with-hashmap-optimized)

### Approach 1: Doubly Linked List with HashMap (Basic)

In this approach, we use a doubly linked list to maintain the order of use of the cache and a HashMap for quick access to elements. The doubly linked list maintains a list of nodes, each representing a key-value pair. The head of the list is the most recently used, and the tail is the least used.

**Intuition:**
- Use a doubly linked list because it provides constant time access to update and remove nodes.
- Use a HashMap to store references to the nodes of the linked list with the key-value pairs so that we can achieve O(1) time complexity for get and put operations.
- When a key is accessed or added, move it to the head of the list. This denotes that it's the most recently used.
- Upon hitting capacity, remove the node at the tail (least recently used).

```java
import java.util.HashMap;

// Node definition for the doubly linked list
class Node {
    int key;
    int value;
    Node prev;
    Node next;

    public Node(int key, int value) {
        this.key = key;
        this.value = value;
    }
}

public class LRUCache {
    private int capacity;
    private HashMap<Integer, Node> map;
    private Node head, tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new HashMap<>();
        this.head = new Node(0, 0);
        this.tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            remove(node);
            insert(node);
            return node.value;
        }
        return -1;
    }

    public void put(int key, int value) {
        if (map.containsKey(key)) {
            remove(map.get(key));
        }
        if (map.size() == capacity) {
            map.remove(tail.prev.key);
            remove(tail.prev);
        }
        Node node = new Node(key, value);
        insert(node);
        map.put(key, node);
    }

    // Removes a node from the doubly linked list
    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    // Inserts a node right after the head
    private void insert(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }
}
```

**Time Complexity:** O(1) for both `get` and `put` operations due to the constant time for hash table and linked list operations.

**Space Complexity:** O(capacity) for storing the elements in the cache.

### Approach 2: Doubly Linked List with HashMap (Optimized)

The second approach involves the same data structures but focuses on optimizing memory usage and cleaning up code for efficiency.

**Key Differences:**
- Consolidate the insert and remove methods for more concise code.
- Ensure edge case handling for all corner scenarios in insert and removal of nodes.

```java
import java.util.HashMap;

class LRUOptimizedCache {
    private HashMap<Integer, Node> cache;
    private int capacity;
    private Node head, tail;

    public LRUOptimizedCache(int capacity) {
        this.capacity = capacity;
        this.cache = new HashMap<>();
        this.head = new Node(0, 0);
        this.tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        Node node = cache.get(key);
        if (node == null) return -1;

        moveToHead(node);
        return node.value;
    }

    public void put(int key, int value) {
        Node node = cache.get(key);
        if (node != null) {
            node.value = value;
            moveToHead(node);
        } else {
            Node newNode = new Node(key, value);
            cache.put(key, newNode);
            addNode(newNode);

            if (cache.size() > capacity) {
                Node tail = popTail();
                cache.remove(tail.key);
            }
        }
    }

    private void addNode(Node node) {
        node.prev = head;
        node.next = head.next;

        head.next.prev = node;
        head.next = node;
    }

    private void removeNode(Node node) {
        Node prev = node.prev;
        Node next = node.next;

        prev.next = next;
        next.prev = prev;
    }

    private void moveToHead(Node node) {
        removeNode(node);
        addNode(node);
    }

    private Node popTail() {
        Node res = tail.prev;
        removeNode(res);
        return res;
    }
}
```

**Time Complexity:** O(1) for both `get` and `put` operations as we continue to have constant time hash table and linked list operations.

**Space Complexity:** O(capacity) for storing the elements and linked list pointers.

