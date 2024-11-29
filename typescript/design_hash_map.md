# [706. Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approach 1: Bucket Array with Chaining

### Solution
typescript
```typescript
// Time Complexity:
// - put(): O(1) on average, O(n) in worst case for resizing
// - get(): O(1) on average, O(n) in worst case
// - remove(): O(1) on average, O(n) in worst case
// Space Complexity: O(n) where n is the number of unique keys

class MyHashMap {
    private class Pair {
        key: number;
        value: number;
        
        constructor(key: number, value: number) {
            this.key = key;
            this.value = value;
        }
    }

    private readonly SIZE: number = 1000; // Number of buckets
    private buckets: Pair[][];

    constructor() {
        this.buckets = new Array(this.SIZE).fill(null).map(() => []);
    }

    private getBucketIndex(key: number): number {
        return key % this.SIZE; // Hash function to determine bucket
    }

    put(key: number, value: number): void {
        const bucketIndex = this.getBucketIndex(key);
        const bucket = this.buckets[bucketIndex];
        for (let pair of bucket) {
            if (pair.key === key) {
                pair.value = value; // Update value if key exists
                return;
            }
        }
        bucket.push(new Pair(key, value)); // Add new key-value pair
    }

    get(key: number): number {
        const bucketIndex = this.getBucketIndex(key);
        const bucket = this.buckets[bucketIndex];
        for (let pair of bucket) {
            if (pair.key === key) {
                return pair.value; // Return value if key exists
            }
        }
        return -1; // Key does not exist
    }

    remove(key: number): void {
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
typescript
```typescript
// Time Complexity:
// - put(): O(n) in worst case for probing
// - get(): O(n) in worst case for probing
// - remove(): O(n) in worst case for probing
// Space Complexity: O(n)

class MyHashMap {
    private map: number[];
    private readonly SIZE: number = 1000001; // Fixed size to handle constraints

    constructor() {
        this.map = new Array(this.SIZE).fill(-1); // Initialize all values to -1 (indicating no key)
    }

    put(key: number, value: number): void {
        this.map[key] = value; // Directly set the value at the index
    }

    get(key: number): number {
        return this.map[key]; // Return the value at the index
    }

    remove(key: number): void {
        this.map[key] = -1; // Reset the value to -1 to indicate removal
    }
}
```

