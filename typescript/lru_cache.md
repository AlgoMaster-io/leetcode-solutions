# 146. [LRU Cache](https://leetcode.com/problems/lru-cache/)

## Approach: HashMap with Doubly Linked List

### Solution
typescript
```typescript
// Time Complexity: O(1) for both get and put operations
// Space Complexity: O(capacity), where capacity is the maximum number of entries in the cache

class LRUCache {
    private class Node {
        key: number;
        value: number;
        prev: Node | null;
        next: Node | null;
        constructor(key: number, value: number) {
            this.key = key;
            this.value = value;
            this.prev = null;
            this.next = null;
        }
    }

    private capacity: number;
    private cache: Map<number, Node>;
    private head: Node;
    private tail: Node;

    constructor(capacity: number) {
        this.capacity = capacity;
        this.cache = new Map<number, Node>();
        this.head = new Node(-1, -1); // Dummy head node
        this.tail = new Node(-1, -1); // Dummy tail node
        this.head.next = this.tail;
        this.tail.prev = this.head;
    }

    get(key: number): number {
        if (!this.cache.has(key)) {
            return -1;
        }
        const node = this.cache.get(key)!;
        this.remove(node); // Move the accessed node to the front
        this.insertToFront(node);
        return node.value;
    }

    put(key: number, value: number): void {
        if (this.cache.has(key)) {
            const node = this.cache.get(key)!;
            this.remove(node); // Remove the old node
        } else if (this.cache.size === this.capacity) {
            // Evict the least recently used node
            this.cache.delete(this.tail.prev!.key);
            this.remove(this.tail.prev!);
        }

        const newNode = new Node(key, value);
        this.cache.set(key, newNode);
        this.insertToFront(newNode);
    }

    // Helper method to remove a node from the linked list
    private remove(node: Node): void {
        node.prev!.next = node.next;
        node.next!.prev = node.prev;
    }

    // Helper method to insert a node at the front of the linked list
    private insertToFront(node: Node): void {
        node.next = this.head.next;
        node.prev = this.head;
        this.head.next!.prev = node;
        this.head.next = node;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * let obj = new LRUCache(capacity);
 * let param_1 = obj.get(key);
 * obj.put(key, value);
 */
```

