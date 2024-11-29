# [706. Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approach 1: Bucket Array with Chaining

### Solution
```javascript
// Time Complexity:
// - put(): O(1) on average, O(n) in worst case for resizing
// - get(): O(1) on average, O(n) in worst case
// - remove(): O(1) on average, O(n) in worst case
// Space Complexity: O(n) where n is the number of unique keys

class MyHashMap {
    constructor() {
        this.SIZE = 1000; // Number of buckets
        this.buckets = new Array(this.SIZE).fill(null).map(() => []);
    }

    getBucketIndex(key) {
        return key % this.SIZE; // Hash function to determine bucket
    }

    put(key, value) {
        const bucketIndex = this.getBucketIndex(key);
        const bucket = this.buckets[bucketIndex];
        for (let pair of bucket) {
            if (pair.key === key) {
                pair.value = value; // Update value if key exists
                return;
            }
        }
        bucket.push({ key, value }); // Add new key-value pair
    }

    get(key) {
        const bucketIndex = this.getBucketIndex(key);
        const bucket = this.buckets[bucketIndex];
        for (let pair of bucket) {
            if (pair.key === key) {
                return pair.value; // Return value if key exists
            }
        }
        return -1; // Key does not exist
    }

    remove(key) {
        const bucketIndex = this.getBucketIndex(key);
        const bucket = this.buckets[bucketIndex];
        for (let i = 0; i < bucket.length; i++) {
            if (bucket[i].key === key) {
                bucket.splice(i, 1); // Remove key-value pair
                return;
            }
        }
    }
}
```

## Approach 2: Open Addressing (Linear Probing)

### Solution
```javascript
// Time Complexity:
// - put(): O(n) in worst case for probing
// - get(): O(n) in worst case for probing
// - remove(): O(n) in worst case for probing
// Space Complexity: O(n)

class MyHashMap {
    constructor() {
        this.SIZE = 1000001; // Fixed size to handle constraints
        this.map = new Array(this.SIZE).fill(-1); // Initialize all values to -1 (indicating no key)
    }

    put(key, value) {
        this.map[key] = value; // Directly set the value at the index
    }

    get(key) {
        return this.map[key]; // Return the value at the index
    }

    remove(key) {
        this.map[key] = -1; // Reset the value to -1 to indicate removal
    }
}
```

