# [Leetcode Problem 706: Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Table of Contents
- [Approach 1: Direct Address Table](#approach-1-direct-address-table)
- [Approach 2: Separate Chaining (Linked List)](#approach-2-separate-chaining-linked-list)
- [Approach 3: Open Addressing](#approach-3-open-addressing)

## Approach 1: Direct Address Table

### Intuition
The naive approach for implementing a hashmap is to use a large array (direct address table) to store values at indices corresponding to the keys directly. This approach assumes that the key range is known and small enough to be manageable in memory.

### Code

```go
type MyHashMap struct {
    data []int
}

const SIZE = 1000001  // The range of keys is [0, 1000000]

func Constructor() MyHashMap {
    // Initialize the map with -1 to signify empty entries
    return MyHashMap{data: make([]int, SIZE)}
}

func (this *MyHashMap) Put(key int, value int)  {
    // Directly map the key to an index and store the value
    this.data[key] = value
}

func (this *MyHashMap) Get(key int) int {
    // Find the value at the index corresponding to the key
    return this.data[key]
}

func (this *MyHashMap) Remove(key int)  {
    // Set the value to -1 to signify an empty slot
    this.data[key] = -1
}
```

### Complexity
- **Time Complexity**: O(1) for all operations - insert, delete, and search.
- **Space Complexity**: O(n), where n is the size of the array (1000000).

## Approach 2: Separate Chaining (Linked List)

### Intuition
In this approach, we use a fixed-size array of buckets each of which will contain a linked list. We'll hash the keys to determine which bucket to store the key-value pairs and handle collisions using chaining.

### Code

```go
type Node struct {
    key, value int
    next *Node
}

type MyHashMap struct {
    buckets []*Node
}

const BUCKET_SIZE = 1000

func Constructor() MyHashMap {
    return MyHashMap{buckets: make([]*Node, BUCKET_SIZE)}
}

func (this *MyHashMap) hash(key int) int {
    return key % BUCKET_SIZE
}

func (this *MyHashMap) Put(key int, value int) {
    hashKey := this.hash(key)
    if this.buckets[hashKey] == nil {
        this.buckets[hashKey] = &Node{key: -1, value: -1}
    }
    prev := this.findNode(hashKey, key)
    if prev.next == nil {
        // Insert new node if key does not exist
        prev.next = &Node{key: key, value: value}
    } else {
        // Update existing node value
        prev.next.value = value
    }
}

func (this *MyHashMap) Get(key int) int {
    hashKey := this.hash(key)
    if this.buckets[hashKey] == nil {
        return -1
    }
    prev := this.findNode(hashKey, key)
    if prev.next == nil {
        return -1
    }
    return prev.next.value
}

func (this *MyHashMap) Remove(key int) {
    hashKey := this.hash(key)
    if this.buckets[hashKey] == nil {
        return
    }
    prev := this.findNode(hashKey, key)
    if prev.next == nil {
        return
    }
    // Remove node from the linked list
    prev.next = prev.next.next
}

func (this *MyHashMap) findNode(hashKey, key int) *Node {
    node := this.buckets[hashKey]
    for node.next != nil && node.next.key != key {
        node = node.next
    }
    return node
}
```

### Complexity
- **Time Complexity**: O(n/k) ≈ O(1), where n is the number of keys and k is the number of buckets. In average scenarios, operations take O(1) time.
- **Space Complexity**: O(n) to store the key-value pairs.

## Approach 3: Open Addressing

### Intuition
In open addressing, all entry records are stored in the array itself and in case of collision, the entry is stored in the next available empty bucket. The probe sequence is determined by a specific strategy (e.g., linear probing, quadratic probing).

We skip implementing this method as it is relatively complex and can lead to performance issues due to clustering and the need to handle deletions carefully, which can undermine simplicity in implementation and understanding. The separate chaining approach is often preferred in educational and practical settings. However, for completeness, open addressing is an option worth studying further in performance-critical applications needing constant space.

