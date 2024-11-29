# [706. Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approach 1: Bucket Array with Chaining

### Solution
```cpp
// Time Complexity:
// - put(): O(1) on average, O(n) in worst case for resizing
// - get(): O(1) on average, O(n) in worst case
// - remove(): O(1) on average, O(n) in worst case
// Space Complexity: O(n) where n is the number of unique keys

#include <list>
#include <vector>

class MyHashMap {
private:
    class Pair {
    public:
        int key, value;
        Pair(int key, int value) : key(key), value(value) {}
    };

    const int SIZE = 1000; // Number of buckets
    std::vector<std::list<Pair>> buckets;

    int getBucketIndex(int key) {
        return key % SIZE; // Hash function to determine bucket
    }

public:
    MyHashMap() : buckets(SIZE) {}

    void put(int key, int value) {
        int bucketIndex = getBucketIndex(key);
        auto& bucket = buckets[bucketIndex];
        for (auto& pair : bucket) {
            if (pair.key == key) {
                pair.value = value; // Update value if key exists
                return;
            }
        }
        bucket.emplace_back(key, value); // Add new key-value pair
    }

    int get(int key) {
        int bucketIndex = getBucketIndex(key);
        auto& bucket = buckets[bucketIndex];
        for (auto& pair : bucket) {
            if (pair.key == key) {
                return pair.value; // Return value if key exists
            }
        }
        return -1; // Key does not exist
    }

    void remove(int key) {
        int bucketIndex = getBucketIndex(key);
        auto& bucket = buckets[bucketIndex];
        for (auto it = bucket.begin(); it != bucket.end(); ++it) {
            if (it->key == key) {
                bucket.erase(it); // Remove key-value pair
                return;
            }
        }
    }
};
```

## Approach 2: Open Addressing (Linear Probing)

### Solution
```cpp
// Time Complexity:
// - put(): O(1) on average, O(n) in worst case for probing
// - get(): O(1) on average, O(n) in worst case for probing
// - remove(): O(1) on average, O(n) in worst case for probing
// Space Complexity: O(n)

class MyHashMap {
private:
    std::vector<int> map;
    const int SIZE = 1000001; // Fixed size to handle constraints

public:
    MyHashMap() : map(SIZE, -1) {} // Initialize all values to -1

    void put(int key, int value) {
        map[key] = value; // Directly set the value at the index
    }

    int get(int key) {
        return map[key]; // Return the value at the index
    }

    void remove(int key) {
        map[key] = -1; // Reset the value to -1 to indicate removal
    }
};
```

