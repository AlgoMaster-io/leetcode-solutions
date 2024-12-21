# [Leetcode 706: Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approaches
1. [Naive Approach: Array of Pairs](#naive-approach-array-of-pairs)
2. [Optimized Approach: Array of Buckets with Linked List Chaining](#optimized-approach-array-of-buckets-with-linked-list-chaining)

---

## Naive Approach: Array of Pairs

### Intuition
The simplest way to implement a hashmap is to use an array of pairs. For each key-value pair, we simply append it to the array. To find a value for a given key, iterate over the list until the key is found. For removing a key, we remove the pair from the array.

### Implementation

```javascript
class MyHashMap {
    constructor() {
        // An array to store key-value pairs
        this.map = [];
    }
    
    put(key, value) {
        // Check if the key already exists and update it
        for (let i = 0; i < this.map.length; i++) {
            if (this.map[i][0] === key) {
                this.map[i][1] = value;
                return;
            }
        }
        // If the key doesn't exist, add a new pair
        this.map.push([key, value]);
    }
    
    get(key) {
        // Look for the key in the array and return its corresponding value
        for (let [k, v] of this.map) {
            if (k === key) return v;
        }
        // Return -1 if the key is not found
        return -1;
    }
    
    remove(key) {
        // Find the key and remove it from the array
        for (let i = 0; i < this.map.length; i++) {
            if (this.map[i][0] === key) {
                this.map.splice(i, 1);
                return;
            }
        }
    }
}
```

### Complexity Analysis
- **Time Complexity**:
  - `put()`, `get()`, `remove()`: O(n) in the worst case where `n` is the number of entries due to linear search.

- **Space Complexity**: O(n), where `n` is the number of key-value pairs stored.

---

## Optimized Approach: Array of Buckets with Linked List Chaining

### Intuition
To optimize the naive approach, we can use a hash function to map keys to indices of an array. Each index in the array, called a "bucket," will contain a linked list of key-value pairs. This approach reduces the average time complexity for operations because it distributes keys uniformly across buckets.

### Implementation

```javascript
class Node {
    constructor(key, value, next = null) {
        this.key = key;
        this.value = value;
        this.next = next;
    }
}

class MyHashMap {
    constructor() {
        this.size = 1000;  // Choose a size for the hashmap
        this.buckets = Array.from({length: this.size}, () => null);
    }
    
    _hash(key) {
        // Simple hash function: modulo the length of the buckets
        return key % this.size;
    }
    
    put(key, value) {
        const index = this._hash(key);
        if (this.buckets[index] === null) {
            this.buckets[index] = new Node(key, value);
        } else {
            let current = this.buckets[index];
            while (true) {
                if (current.key === key) {
                    current.value = value; // Update value if key exists
                    return;
                }
                if (current.next === null) break;
                current = current.next;
            }
            current.next = new Node(key, value);  // Add new node at the end
        }
    }
    
    get(key) {
        const index = this._hash(key);
        let current = this.buckets[index];
        while (current) {
            if (current.key === key) return current.value;
            current = current.next;
        }
        return -1;  // Return -1 if key is not found
    }
    
    remove(key) {
        const index = this._hash(key);
        let current = this.buckets[index];
        let prev = null;
        while (current) {
            if (current.key === key) {
                if (prev) {
                    prev.next = current.next;
                } else {
                    this.buckets[index] = current.next;  // Handle removal of head node
                }
                return;
            }
            prev = current;
            current = current.next;
        }
    }
}
```

### Complexity Analysis
- **Time Complexity**:
  - `put()`, `get()`, `remove()`: O(n/k) on average, where `n` is the number of entries, and `k` is the number of buckets. Assuming a good hash function, this is approximately O(1).

- **Space Complexity**: O(n + k), where `n` is the number of stored key-value pairs, and `k` is the number of buckets.

The optimized approach balances time complexity across operations by attempting to distribute keys evenly across buckets using a hashing function. This is a common strategy to improve the efficiency of hashmap operations.

