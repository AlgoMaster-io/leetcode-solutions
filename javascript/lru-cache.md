# [Leetcode 146: LRU Cache](https://leetcode.com/problems/lru-cache/)

## Approaches
- [Approach 1: Brute Force with Map and Array](#approach-1-brute-force-with-map-and-array)
- [Approach 2: Ordered Map (Map and Double Linked List)](#approach-2-ordered-map-map-and-double-linked-list)

### Approach 1: Brute Force with Map and Array

**Intuition**

The problem requires implementing a Least Recently Used (LRU) cache. An LRU cache evicts the least recently used item when it reaches its capacity. One straightforward approach is to use a Map to keep track of the key-value pairs and an array to maintain the order of usage. Whenever a key is accessed, it's moved to the end of the array to mark it as the most recently used.

**Detailed Steps**

1. Use a `Map` to store the key-value pairs.
2. Use an `Array` to maintain the order of the keys based on their usage.
3. When a key is accessed (`get` operation), check if the key exists.
   - If it exists, move the key to the end of the array to show that it has recently been used.
4. When a key is added (`put` operation):
   - If the key already exists, update the value and move the key to the end of the usage array.
   - If the capacity will be exceeded with this addition, remove the first element from the array since it is the least recently used, and delete its corresponding entry from the map.
5. Add the key-value pair to the map and mark the key as recently used by adding it to the end of the array.

**Code**

```javascript
class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.map = new Map();
        this.usage = [];
    }

    get(key) {
        if (!this.map.has(key)) {
            return -1;
        }
        // Move the accessed key to the end of the usage array
        this._makeRecentlyUsed(key);
        return this.map.get(key);
    }

    put(key, value) {
        if (this.map.has(key)) {
            // Update existing key and mark as recently used
            this._makeRecentlyUsed(key);
        } else {
            // If not existing key but at capacity, remove least recently used item
            if (this.usage.length === this.capacity) {
                const leastUsedKey = this.usage.shift();
                this.map.delete(leastUsedKey);
            }
            // Now store the new key
            this.usage.push(key);
        }
        this.map.set(key, value);
    }

    _makeRecentlyUsed(key) {
        const index = this.usage.indexOf(key);
        if (index > -1) {
            this.usage.splice(index, 1);
        }
        this.usage.push(key);
    }
}
```

**Complexity Analysis**

- **Time Complexity**: O(n) for `get` and `put` operations, where n is the number of elements in the usage array. Accessing a key requires linear search in the array.
- **Space Complexity**: O(capacity) due to storing keys in both the map and the array.

### Approach 2: Ordered Map (Map and Double Linked List)

**Intuition**

The above approach is inefficient due to the linear search required to update the usage order. A more optimal way is to use a doubly linked list combined with a `Map`. Think of the linked list as ordered by access time; the most recently accessed node is at the head, and the least recently accessed is at the tail.

**Detailed Steps**

1. Use a doubly linked list to maintain the least and most recent order of usage.
2. Use a `Map` to store the key-node pairs.
3. When a key is accessed (`get` operation):
   - Check if the key exists.
   - Move the accessed node to the head of the list.
4. When a key is added (`put` operation):
   - If the key already exists, simply update the value and move the node to the head.
   - If the cache has reached its capacity, remove the least used node (tail).
   - Add the new node to the head of the list and to the map.

**Code**

```javascript
class Node {
    constructor(key, value) {
        this.key = key;
        this.value = value;
        this.prev = null;
        this.next = null;
    }
}

class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.cache = new Map();
        this.head = new Node(0, 0);
        this.tail = new Node(0, 0);
        this.head.next = this.tail;
        this.tail.prev = this.head;
    }

    _add(node) {
        node.next = this.head.next;
        node.prev = this.head;
        this.head.next.prev = node;
        this.head.next = node;
    }

    _remove(node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    get(key) {
        if (this.cache.has(key)) {
            const node = this.cache.get(key);
            this._remove(node);
            this._add(node);
            return node.value;
        }
        return -1;
    }

    put(key, value) {
        if (this.cache.has(key)) {
            this._remove(this.cache.get(key));
        } else if (this.cache.size === this.capacity) {
            this.cache.delete(this.tail.prev.key);
            this._remove(this.tail.prev);
        }
        const node = new Node(key, value);
        this.cache.set(key, node);
        this._add(node);
    }
}
```

**Complexity Analysis**

- **Time Complexity**: O(1) for `get` and `put` operations. The map provides O(1) access, and the list provides O(1) insert and delete operations.
- **Space Complexity**: O(capacity) for storing keys and nodes. The map and list together store at most 2*capacity nodes in the worst case.

