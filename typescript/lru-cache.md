# [Leetcode 146: LRU Cache](https://leetcode.com/problems/lru-cache/)

## Approaches

- [Approach 1: Using a HashMap to store keys and a separate array to track the order of access](#approach-1-using-a-hashmap-to-store-keys-and-a-separate-array-to-track-the-order-of-access)
- [Approach 2: Using a combination of HashMap and Doubly Linked List](#approach-2-using-a-combination-of-hashmap-and-doubly-linked-list)

---

## Approach 1: Using a HashMap to store keys and a separate array to track the order of access

### Intuition:
One of the simplest ways to implement an LRU Cache is by using a HashMap to store the key-value pairs and an additional array to keep track of the order of access. A key's presence in the array indicates its recency of usage.

### Algorithm:
1. **Maintaining the cache**:
   - Use a `Map` to store the key-value pairs.
   - Use an `Array` to maintain the order of access.

2. **Get operation**:
   - If the key is present in the map, update its position in the array to mark it as recently used and return the value.

3. **Put operation**:
   - If the key is already in the map, update the value and mark the key as recently used by adjusting its position in the array.
   - If not, first check if the size of the map exceeds the capacity.
     - If it does, remove the oldest entry (i.e., the first element in the array) from the map and the array.
   - Add the new key-value pair to the map and mark the key as recently used by placing it at the end of the array.

### Code:

```typescript
class LRUCache {
    private capacity: number;
    private cache: Map<number, number>;
    private order: number[];

    constructor(capacity: number) {
        this.capacity = capacity;
        this.cache = new Map();
        this.order = [];
    }

    get(key: number): number {
        // Check if the key exists in the cache
        if (!this.cache.has(key)) return -1;
        
        // Move the accessed key to the end to mark it as recently used
        const index = this.order.indexOf(key);
        if (index > -1) {
            this.order.splice(index, 1);
        }
        this.order.push(key);
        
        // Return the value associated with the key
        return this.cache.get(key)!;
    }

    put(key: number, value: number): void {
        // If the key exists, update the value and move it to recent
        if (this.cache.has(key)) {
            const index = this.order.indexOf(key);
            if (index > -1) {
                this.order.splice(index, 1);
            }
        } else {
            // If the key does not exist and capacity is exceeded
            if (this.cache.size >= this.capacity) {
                const oldestKey = this.order.shift()!;
                this.cache.delete(oldestKey);
            }
        }
        // Add or update the key-value pair
        this.cache.set(key, value);
        this.order.push(key);
    }
}
```

### Complexity Analysis:
- **Time Complexity**: O(n) for both `get` and `put` operations due to the shifting and splicing operations carried out on the order array.
- **Space Complexity**: O(capacity), as we only store the keys in the map and array.

---

## Approach 2: Using a combination of HashMap and Doubly Linked List

### Intuition:
A more optimal solution uses a HashMap in combination with a Doubly Linked List. The HashMap aids in fast retrieval of data, while the Doubly Linked List allows maintaining the order of access efficiently.

### Algorithm:
1. **Doubly Linked List**:
   - Nodes contain key-value pairs.
   - Provides efficient insertion and deletion from both ends.

2. **HashMap**:
   - Maps keys to nodes in the doubly linked list for quick access.

3. **Get operation**:
   - Retrieve from HashMap and move the node to the head of the list to mark as recently used.

4. **Put operation**:
   - If the key is present, update its value and move its node to the head of the list.
   - If not, create a new node. If capacity is exceeded, remove the node at the tail of the list.
   - Add the new node at the head of the list.

### Code:

```typescript
type Node = {
    key: number,
    value: number,
    prev: Node | null,
    next: Node | null
};

class LRUCache {
    private capacity: number;
    private map: Map<number, Node>;
    private head: Node;
    private tail: Node;

    constructor(capacity: number) {
        this.capacity = capacity;
        
        // Create dummy head and tail nodes
        this.head = { key: 0, value: 0, prev: null, next: null };
        this.tail = { key: 0, value: 0, prev: null, next: null };
        
        // Link head and tail
        this.head.next = this.tail;
        this.tail.prev = this.head;
        
        this.map = new Map();
    }

    get(key: number): number {
        if (this.map.has(key)) {
            const node = this.map.get(key)!;
            this.remove(node);
            this.insert(node);
            return node.value;
        }
        return -1;
    }

    put(key: number, value: number): void {
        if (this.map.has(key)) {
            const node = this.map.get(key)!;
            node.value = value;
            this.remove(node);
            this.insert(node);
        } else {
            if (this.map.size >= this.capacity) {
                const lru = this.tail.prev!;
                this.remove(lru);
                this.map.delete(lru.key);
            }
            const newNode = { key, value, prev: null, next: null };
            this.insert(newNode);
            this.map.set(key, newNode);
        }
    }

    private remove(node: Node): void {
        const { prev, next } = node;
        if (prev) prev.next = next;
        if (next) next.prev = prev;
    }

    private insert(node: Node): void {
        node.next = this.head.next;
        node.prev = this.head;
        if (this.head.next) this.head.next.prev = node;
        this.head.next = node;
    }
}
```

### Complexity Analysis:
- **Time Complexity**: O(1) for both `get` and `put` operations due to efficient look-up, insertion, and removal provided by the combined use of a hashmap and doubly linked list.
- **Space Complexity**: O(capacity), as we store only the keys within the capacity.

