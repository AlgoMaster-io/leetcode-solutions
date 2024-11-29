# [706. Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approach 1: Bucket Array with Chaining

### Solution

```csharp
// Time Complexity:
// - put(): O(1) on average, O(n) in worst case for resizing
// - get(): O(1) on average, O(n) in worst case
// - remove(): O(1) on average, O(n) in worst case
// Space Complexity: O(n) where n is the number of unique keys

using System.Collections.Generic;

public class MyHashMap {
    private class Pair {
        public int Key { get; set; }
        public int Value { get; set; }
        public Pair(int key, int value) {
            Key = key;
            Value = value;
        }
    }

    private const int SIZE = 1000; // Number of buckets
    private readonly LinkedList<Pair>[] buckets;

    public MyHashMap() {
        buckets = new LinkedList<Pair>[SIZE];
        for (int i = 0; i < SIZE; i++) {
            buckets[i] = new LinkedList<Pair>();
        }
    }

    private int GetBucketIndex(int key) {
        return key % SIZE; // Hash function to determine bucket
    }

    public void Put(int key, int value) {
        int bucketIndex = GetBucketIndex(key);
        LinkedList<Pair> bucket = buckets[bucketIndex];
        foreach (Pair pair in bucket) {
            if (pair.Key == key) {
                pair.Value = value; // Update value if key exists
                return;
            }
        }
        bucket.AddLast(new Pair(key, value)); // Add new key-value pair
    }

    public int Get(int key) {
        int bucketIndex = GetBucketIndex(key);
        LinkedList<Pair> bucket = buckets[bucketIndex];
        foreach (Pair pair in bucket) {
            if (pair.Key == key) {
                return pair.Value; // Return value if key exists
            }
        }
        return -1; // Key does not exist
    }

    public void Remove(int key) {
        int bucketIndex = GetBucketIndex(key);
        LinkedList<Pair> bucket = buckets[bucketIndex];
        foreach (Pair pair in bucket) {
            if (pair.Key == key) {
                bucket.Remove(pair); // Remove key-value pair
                return;
            }
        }
    }
}
```

## Approach 2: Open Addressing (Linear Probing)

### Solution

```csharp
// Time Complexity:
// - put(): O(n) in worst case for probing
// - get(): O(n) in worst case for probing
// - remove(): O(n) in worst case for probing
// Space Complexity: O(n)

public class MyHashMap {
    private readonly int[] map;
    private const int SIZE = 1000001; // Fixed size to handle constraints

    public MyHashMap() {
        map = new int[SIZE];
        for (int i = 0; i < SIZE; i++) {
            map[i] = -1; // Initialize all values to -1 (indicating no key)
        }
    }

    public void Put(int key, int value) {
        map[key] = value; // Directly set the value at the index
    }

    public int Get(int key) {
        return map[key]; // Return the value at the index
    }

    public void Remove(int key) {
        map[key] = -1; // Reset the value to -1 to indicate removal
    }
}
```

