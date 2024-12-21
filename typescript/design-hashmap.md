# [Leetcode 706: Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approaches
1. [Basic Approach: Array with Modulo Hash Function](#basic-approach-array-with-modulo-hash-function)
2. [Intermediate Approach: Array of Buckets with Separate Chaining](#intermediate-approach-array-of-buckets-with-separate-chaining)

---

## Basic Approach: Array with Modulo Hash Function

### Intuition
The simplest way to design a hash map is to use a fixed-size array, where each index acts like a bucket for storing key-value pairs. We use a modulo operation on the key to map it to an index in the array. While this solution is simple, it isn't efficient in handling collisions (where different keys map to the same index).

### Algorithm
1. Initialize an array of fixed size.
2. For each key, calculate the index using the modulo operator.
3. Store the value at the calculated index.
4. Handle collisions by simply overwriting previous values (this leads to data being lost if there are collisions).

### Code
```typescript
class MyHashMap {
    private map: number[];
    private static SIZE = 1000; // Array size
    
    constructor() {
        this.map = new Array(MyHashMap.SIZE).fill(-1); // Initialize with -1 to denote empty slots
    }
    
    put(key: number, value: number): void {
        const index = key % MyHashMap.SIZE; // Compute index using the hash function
        this.map[index] = value; // Store the value at the index
    }
    
    get(key: number): number {
        const index = key % MyHashMap.SIZE; // Compute index using the hash function
        return this.map[index]; // Return the value at the index
    }
    
    remove(key: number): void {
        const index = key % MyHashMap.SIZE; // Compute index using the hash function
        this.map[index] = -1; // Reset the slot to -1 indicating removal
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(1) for `put`, `get`, and `remove` operations, because array access takes constant time.
- **Space Complexity**: O(n), where n is the size of the pre-allocated array.

---

## Intermediate Approach: Array of Buckets with Separate Chaining

### Intuition
To handle collisions beyond the basics, we can implement an array of buckets. In each bucket, we store a list of key-value pairs. This ensures that even when multiple keys hash to the same index, each key-value pair is preserved.

### Algorithm
1. Initialize an array to act as the main structure of the hash map.
2. Each entry in this array is a list (or linked list) used to store key-value pairs.
3. For put operations, hash the key to get an index and append key-value pairs to the bucket at that index.
4. For get operations, iterate the bucket at the hashed index to find the key.
5. For remove operations, iterate the bucket to find and remove the key-value pair matching the key.

### Code
```typescript
class MyHashMap {
    private map: Array<[number, number][]>; // array of buckets
    private static SIZE = 1000;
    
    constructor() {
        this.map = new Array(MyHashMap.SIZE).fill(null).map(() => []); // Initialize each bucket as an empty list
    }
    
    put(key: number, value: number): void {
        const index = key % MyHashMap.SIZE; // Calculate bucket index
        const bucket = this.map[index]; // Get the bucket
        for (let i = 0; i < bucket.length; i++) { // Check if key exists in the bucket
            if (bucket[i][0] === key) {
                bucket[i][1] = value; // Update value if key is found
                return;
            }
        }
        bucket.push([key, value]); // Append new key-value pair
    }
    
    get(key: number): number {
        const index = key % MyHashMap.SIZE; // Calculate bucket index
        const bucket = this.map[index]; // Get the bucket
        for (const [k, v] of bucket) { // Search for the key in the bucket
            if (k === key) return v; // Return value if key is found
        }
        return -1; // Key was not found
    }

    remove(key: number): void {
        const index = key % MyHashMap.SIZE; // Calculate bucket index
        const bucket = this.map[index]; // Get the bucket
        for (let i = 0; i < bucket.length; i++) { // Search for the key in the bucket
            if (bucket[i][0] === key) {
                bucket.splice(i, 1); // Remove the key-value pair
                return;
            }
        }
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n/k) for `put`, `get`, and `remove` operations, which can be approximated to O(1) in average case, where n is the number of operations and k is the number of buckets.
- **Space Complexity**: O(n + k), where n is the number of key-value pairs stored and k is the number of buckets.

