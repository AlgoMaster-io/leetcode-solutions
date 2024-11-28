# [706. Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approach 1: Bucket Array with Chaining

### Solution
```java
// Time Complexity:
// - put(): O(1) on average, O(n) in worst case for resizing
// - get(): O(1) on average, O(n) in worst case
// - remove(): O(1) on average, O(n) in worst case
// Space Complexity: O(n) where n is the number of unique keys

import java.util.LinkedList;

class MyHashMap {
    private class Pair {
        int key, value;
        public Pair(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private final int SIZE = 1000; // Number of buckets
    private LinkedList<Pair>[] buckets;

    public MyHashMap() {
        buckets = new LinkedList[SIZE];
        for (int i = 0; i < SIZE; i++) {
            buckets[i] = new LinkedList<>();
        }
    }

    private int getBucketIndex(int key) {
        return key % SIZE; // Hash function to determine bucket
    }

    public void put(int key, int value) {
        int bucketIndex = getBucketIndex(key);
        LinkedList<Pair> bucket = buckets[bucketIndex];
        for (Pair pair : bucket) {
            if (pair.key == key) {
                pair.value = value; // Update value if key exists
                return;
            }
        }
        bucket.add(new Pair(key, value)); // Add new key-value pair
    }

    public int get(int key) {
        int bucketIndex = getBucketIndex(key);
        LinkedList<Pair> bucket = buckets[bucketIndex];
        for (Pair pair : bucket) {
            if (pair.key == key) {
                return pair.value; // Return value if key exists
            }
        }
        return -1; // Key does not exist
    }

    public void remove(int key) {
        int bucketIndex = getBucketIndex(key);
        LinkedList<Pair> bucket = buckets[bucketIndex];
        for (Pair pair : bucket) {
            if (pair.key == key) {
                bucket.remove(pair); // Remove key-value pair
                return;
            }
        }
    }
}
```

## Approach 2: Open Addressing (Linear Probing)

### Solution
```java
// Time Complexity:
// - put(): O(n) in worst case for probing
// - get(): O(n) in worst case for probing
// - remove(): O(n) in worst case for probing
// Space Complexity: O(n)

class MyHashMap {
    private int[] map;
    private final int SIZE = 1000001; // Fixed size to handle constraints

    public MyHashMap() {
        map = new int[SIZE];
        for (int i = 0; i < SIZE; i++) {
            map[i] = -1; // Initialize all values to -1 (indicating no key)
        }
    }

    public void put(int key, int value) {
        map[key] = value; // Directly set the value at the index
    }

    public int get(int key) {
        return map[key]; // Return the value at the index
    }

    public void remove(int key) {
        map[key] = -1; // Reset the value to -1 to indicate removal
    }
}
```