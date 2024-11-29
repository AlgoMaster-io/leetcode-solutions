# 146. [LRU Cache](https://leetcode.com/problems/lru-cache/)

## Approach: HashMap with Doubly Linked List

### Solution
```java
// Time Complexity: O(1) for both get and put operations
// Space Complexity: O(capacity), where capacity is the maximum number of entries in the cache
import java.util.HashMap;

class LRUCache {
    private class Node {
        int key, value;
        Node prev, next;
        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private final int capacity;
    private final HashMap<Integer, Node> cache;
    private final Node head, tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        cache = new HashMap<>();
        head = new Node(-1, -1); // Dummy head node
        tail = new Node(-1, -1); // Dummy tail node
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        if (!cache.containsKey(key)) {
            return -1;
        }
        Node node = cache.get(key);
        remove(node); // Move the accessed node to the front
        insertToFront(node);
        return node.value;
    }

    public void put(int key, int value) {
        if (cache.containsKey(key)) {
            Node node = cache.get(key);
            remove(node); // Remove the old node
        } else if (cache.size() == capacity) {
            // Evict the least recently used node
            cache.remove(tail.prev.key);
            remove(tail.prev);
        }

        Node newNode = new Node(key, value);
        cache.put(key, newNode);
        insertToFront(newNode);
    }

    // Helper method to remove a node from the linked list
    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    // Helper method to insert a node at the front of the linked list
    private void insertToFront(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key, value);
 */
```