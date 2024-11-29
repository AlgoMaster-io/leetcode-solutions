# 146. [LRU Cache](https://leetcode.com/problems/lru-cache/)

## Approach: HashMap with Doubly Linked List

### Solution
```javascript
// Time Complexity: O(1) for both get and put operations
// Space Complexity: O(capacity), where capacity is the maximum number of entries in the cache

class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.cache = new Map();
        this.head = { key: -1, value: -1, prev: null, next: null }; // Dummy head node
        this.tail = { key: -1, value: -1, prev: null, next: null }; // Dummy tail node
        this.head.next = this.tail;
        this.tail.prev = this.head;
    }

    get(key) {
        if (!this.cache.has(key)) {
            return -1;
        }

        const node = this.cache.get(key);
        this._remove(node); // Move the accessed node to the front
        this._insertToFront(node);

        return node.value;
    }

    put(key, value) {
        if (this.cache.has(key)) {
            const existingNode = this.cache.get(key);
            this._remove(existingNode); // Remove the old node
        } else if (this.cache.size === this.capacity) {
            // Evict the least recently used node
            const lruNode = this.tail.prev;
            this.cache.delete(lruNode.key);
            this._remove(lruNode);
        }

        const newNode = { key, value, prev: null, next: null };
        this.cache.set(key, newNode);
        this._insertToFront(newNode);
    }

    // Helper method to remove a node from the linked list
    _remove(node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    // Helper method to insert a node at the front of the linked list
    _insertToFront(node) {
        node.next = this.head.next;
        node.prev = this.head;
        this.head.next.prev = node;
        this.head.next = node;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity);
 * var param_1 = obj.get(key);
 * obj.put(key, value);
 */
```

