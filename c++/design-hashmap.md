## [Leetcode Problem 706: Design HashMap](https://leetcode.com/problems/design-hashmap/)

### Approaches
- [Approach 1: Array-Based HashMap](#approach-1-array-based-hashmap)
- [Approach 2: Chaining using Linked Lists](#approach-2-chaining-using-linked-lists)
- [Approach 3: Open Addressing (Linear Probing)](#approach-3-open-addressing-linear-probing)

---

### Approach 1: Array-Based HashMap

#### Intuition
The most straightforward implementation of a hashmap can be done using an array. You can allocate a fixed size array and use indices as keys. This approach is simple and assumes the keys are within a pre-defined range.

1. **Array Size Limitation:** Since the problem has limits on the values of keys (0 to 1000000), we can create an array of size 1000001.
2. **Direct Access:** We use the direct index corresponding to the key to store the value.

#### Code

```cpp
class MyHashMap {
private:
    std::vector<int> hashmap;
    const int MAX_SIZE = 1000001; // As keys are from 0 to 1000000

public:
    /** Initialize your data structure here. */
    MyHashMap() : hashmap(MAX_SIZE, -1) {}

    /** value will always be non-negative. */
    void put(int key, int value) {
        // Store the value at the index equivalent to the key
        hashmap[key] = value;
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        // Return the value at index 'key'; return -1 if not present
        return hashmap[key];
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        // Set the value at index 'key' as -1 indicating that it's removed
        hashmap[key] = -1;
    }
};
```

#### Complexity Analysis
- **Time Complexity:** O(1) for `put`, `get`, and `remove`, as it directly accesses an array index.
- **Space Complexity:** O(N) where N is the range of possible keys (1000001).

---

### Approach 2: Chaining using Linked Lists

#### Intuition
When the key space is large and we want a more flexible solution with fewer memory constraints, we can use separate chaining with linked lists to handle collisions.

1. **Buckets:** We divide the whole possible range of keys into several buckets.
2. **Linked List:** Each bucket will maintain a linked list of (key, value) pairs to handle hash collisions.

#### Code

```cpp
class MyHashMap {
private:
    static const int NUM_BUCKETS = 769; // Using a prime number to distribute keys uniformly
    std::vector<std::list<std::pair<int, int>>> buckets;

    int getHash(int key) {
        return key % NUM_BUCKETS;
    }

public:
    /** Initialize your data structure here. */
    MyHashMap() : buckets(NUM_BUCKETS) {}

    /** value will always be non-negative. */
    void put(int key, int value) {
        int hashKey = getHash(key);
        auto& chain = buckets[hashKey];
        for (auto& [k, v] : chain) {
            if (k == key) {
                v = value;
                return;
            }
        }
        chain.push_back({key, value});
    }

    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int hashKey = getHash(key);
        const auto& chain = buckets[hashKey];
        for (const auto& [k, v] : chain) {
            if (k == key) {
                return v;
            }
        }
        return -1;
    }

    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int hashKey = getHash(key);
        auto& chain = buckets[hashKey];
        chain.remove_if([key](const std::pair<int, int>& kv) { return kv.first == key; });
    }
};
```

#### Complexity Analysis
- **Time Complexity:** Average O(1) for `put`, `get`, and `remove`, but it could degrade to O(N) in worst-case scenarios where all keys fall into the same bucket.
- **Space Complexity:** O(N + M) where N is the range of keys and M is the number of buckets.

---

### Approach 3: Open Addressing (Linear Probing)

#### Intuition
Instead of using linked lists to handle collisions, open addressing uses probing. When a collision occurs, it finds the next free slot in the array using a specific probing sequence (e.g., linear probing).

1. **Probing:** In linear probing, if the desired position is occupied, try the next position.
2. **Resizing:** Implement resizing strategies to handle load factors.

#### Code

This approach is generally more complex to implement and not shown here due to its complexity.

#### Complexity Analysis
- **Time Complexity:** O(1) on average for `put`, `get`, and `remove`. In a bad scenario, it can degrade to O(N).
- **Space Complexity:** O(N).

Each approach has its strengths and weaknesses depending on the trade-offs you're willing to consider between simplicity, efficiency, and memory use.

