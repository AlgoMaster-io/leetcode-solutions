# [706. Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approach 1: Bucket Array with Chaining

### Solution
python
```python
# Time Complexity:
# - put(): O(1) on average, O(n) in worst case for resizing
# - get(): O(1) on average, O(n) in worst case
# - remove(): O(1) on average, O(n) in worst case
# Space Complexity: O(n) where n is the number of unique keys

class MyHashMap:

    class Pair:
        def __init__(self, key, value):
            self.key = key
            self.value = value

    def __init__(self):
        self.SIZE = 1000  # Number of buckets
        self.buckets = [[] for _ in range(self.SIZE)]

    def get_bucket_index(self, key):
        return key % self.SIZE  # Hash function to determine bucket

    def put(self, key, value):
        bucket_index = self.get_bucket_index(key)
        bucket = self.buckets[bucket_index]
        for pair in bucket:
            if pair.key == key:
                pair.value = value  # Update value if key exists
                return
        bucket.append(self.Pair(key, value))  # Add new key-value pair

    def get(self, key):
        bucket_index = self.get_bucket_index(key)
        bucket = self.buckets[bucket_index]
        for pair in bucket:
            if pair.key == key:
                return pair.value  # Return value if key exists
        return -1  # Key does not exist

    def remove(self, key):
        bucket_index = self.get_bucket_index(key)
        bucket = self.buckets[bucket_index]
        for pair in bucket:
            if pair.key == key:
                bucket.remove(pair)  # Remove key-value pair
                return
```

## Approach 2: Open Addressing (Linear Probing)

### Solution
python
```python
# Time Complexity:
# - put(): O(n) in worst case for probing
# - get(): O(n) in worst case for probing
# - remove(): O(n) in worst case for probing
# Space Complexity: O(n)

class MyHashMap:

    def __init__(self):
        self.SIZE = 1000001  # Fixed size to handle constraints
        self.map = [-1] * self.SIZE  # Initialize map with -1 denoting no key

    def put(self, key, value):
        self.map[key] = value  # Directly set the value at the index

    def get(self, key):
        return self.map[key]  # Return the value at the index

    def remove(self, key):
        self.map[key] = -1  # Reset the value to -1 to indicate removal
```

