# 146. [LRU Cache](https://leetcode.com/problems/lru-cache/)

## Approach: HashMap with Doubly Linked List

### Solution
csharp
```csharp
// Time Complexity: O(1) for both get and put operations
// Space Complexity: O(capacity), where capacity is the maximum number of entries in the cache
using System.Collections.Generic;

public class LRUCache {
    private class Node {
        public int Key, Value;
        public Node Prev, Next;
        public Node(int key, int value) {
            Key = key;
            Value = value;
        }
    }

    private readonly int capacity;
    private readonly Dictionary<int, Node> cache;
    private readonly Node head, tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        cache = new Dictionary<int, Node>();
        head = new Node(-1, -1); // Dummy head node
        tail = new Node(-1, -1); // Dummy tail node
        head.Next = tail;
        tail.Prev = head;
    }

    public int Get(int key) {
        if (!cache.ContainsKey(key)) {
            return -1;
        }
        Node node = cache[key];
        Remove(node); // Move the accessed node to the front
        InsertToFront(node);
        return node.Value;
    }

    public void Put(int key, int value) {
        if (cache.ContainsKey(key)) {
            Node node = cache[key];
            Remove(node); // Remove the old node
        } else if (cache.Count == capacity) {
            // Evict the least recently used node
            cache.Remove(tail.Prev.Key);
            Remove(tail.Prev);
        }

        Node newNode = new Node(key, value);
        cache[key] = newNode;
        InsertToFront(newNode);
    }

    // Helper method to remove a node from the linked list
    private void Remove(Node node) {
        node.Prev.Next = node.Next;
        node.Next.Prev = node.Prev;
    }

    // Helper method to insert a node at the front of the linked list
    private void InsertToFront(Node node) {
        node.Next = head.Next;
        node.Prev = head;
        head.Next.Prev = node;
        head.Next = node;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.Get(key);
 * obj.Put(key, value);
 */
```

