# [706. Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approach 1: Bucket Array with Chaining

### Solution

```go
// Time Complexity:
// - put(): O(1) on average, O(n) in worst case for resizing
// - get(): O(1) on average, O(n) in worst case
// - remove(): O(1) on average, O(n) in worst case
// Space Complexity: O(n) where n is the number of unique keys

type Pair struct {
    key, value int
}

type MyHashMap struct {
    size    int
    buckets [][]Pair
}

func Constructor() MyHashMap {
    size := 1000
    buckets := make([][]Pair, size)
    return MyHashMap{size, buckets}
}

func (this *MyHashMap) getBucketIndex(key int) int {
    return key % this.size
}

func (this *MyHashMap) Put(key int, value int) {
    bucketIndex := this.getBucketIndex(key)
    bucket := this.buckets[bucketIndex]
    for i, pair := range bucket {
        if pair.key == key {
            this.buckets[bucketIndex][i].value = value // Update value if key exists
            return
        }
    }
    this.buckets[bucketIndex] = append(this.buckets[bucketIndex], Pair{key, value}) // Add new key-value pair
}

func (this *MyHashMap) Get(key int) int {
    bucketIndex := this.getBucketIndex(key)
    bucket := this.buckets[bucketIndex]
    for _, pair := range bucket {
        if pair.key == key {
            return pair.value // Return value if key exists
        }
    }
    return -1 // Key does not exist
}

func (this *MyHashMap) Remove(key int) {
    bucketIndex := this.getBucketIndex(key)
    bucket := this.buckets[bucketIndex]
    for i, pair := range bucket {
        if pair.key == key {
            this.buckets[bucketIndex] = append(bucket[:i], bucket[i+1:]...) // Remove key-value pair
            return
        }
    }
}
```

## Approach 2: Open Addressing (Linear Probing)

### Solution

```go
// Time Complexity:
// - put(): O(n) in worst case for probing
// - get(): O(n) in worst case for probing
// - remove(): O(n) in worst case for probing
// Space Complexity: O(n)

type MyHashMap struct {
    mapValues []int
    size      int
}

func Constructor() MyHashMap {
    size := 1000001
    mapValues := make([]int, size)
    for i := 0; i < size; i++ {
        mapValues[i] = -1 // Initialize all values to -1 (indicating no key)
    }
    return MyHashMap{mapValues, size}
}

func (this *MyHashMap) Put(key int, value int) {
    this.mapValues[key] = value // Directly set the value at the index
}

func (this *MyHashMap) Get(key int) int {
    return this.mapValues[key] // Return the value at the index
}

func (this *MyHashMap) Remove(key int) {
    this.mapValues[key] = -1 // Reset the value to -1 to indicate removal
}
```

