# [LeetCode 706: Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approaches
- [Approach 1: Using Chaining with Lists](#approach-1-using-chaining-with-lists)
- [Approach 2: Using Open Addressing with Linear Probing](#approach-2-using-open-addressing-with-linear-probing)

### Approach 1: Using Chaining with Lists

**Intuition:**
In this approach, we use a technique called chaining to resolve collisions, which involves each bucket containing a list. When a collision occurs (two keys hash to the same index), we append the new key-value pair to the list at that index.

**Implementation Steps:**
1. Initialize a fixed-size array (the hash table) with `None`.
2. Use a hashing function to map a key to a specific index in the array. In our simple case, we can use `key % size`.
3. For `put`, we check if the key already exists in the list at that index and update it; otherwise, append the key-value pair.
4. For `get`, traverse the list at the computed index and return the value associated with the key.
5. For `remove`, traverse the list at the given index, and remove the entry with the specified key if it exists.

**Code:**
```python
class MyHashMap:
    def __init__(self):
        # Initializing the hash table size moderately to 1000
        self.size = 1000
        # Create a list of empty lists for maintaining chains for each index
        self.table = [[] for _ in range(self.size)]

    def _hash(self, key):
        # Define a simple hash function
        return key % self.size

    def put(self, key: int, value: int) -> None:
        # Obtain the hash for the key
        hash_key = self._hash(key)
        # Check if the key already exists and update the value
        for pair in self.table[hash_key]:
            if pair[0] == key:
                pair[1] = value
                return
        # If the key was not present, append the key-value pair
        self.table[hash_key].append([key, value])

    def get(self, key: int) -> int:
        # Obtain the hash for the key
        hash_key = self._hash(key)
        # Check if the key exists in the chain and return the value
        for pair in self.table[hash_key]:
            if pair[0] == key:
                return pair[1]
        # Return -1 if the key doesn't exist
        return -1

    def remove(self, key: int) -> None:
        # Obtain the hash for the key
        hash_key = self._hash(key)
        # Find the key in the chain and remove the entry
        for i, pair in enumerate(self.table[hash_key]):
            if pair[0] == key:
                del self.table[hash_key][i]
                return
```

**Complexity Analysis:**
- Time Complexity: 
  - `put`: O(N/K) where K is the size of the hash table and N is the number of all possible keys. In the best scenario, this approaches O(1).
  - `get`: O(N/K) for similar reasons as put.
  - `remove`: O(N/K).
- Space Complexity: O(N + K) due to storage in lists and the table size.

### Approach 2: Using Open Addressing with Linear Probing

**Intuition:**
Open addressing techniques, like linear probing, place all elements in the hash table directly and resolve collisions by finding the next available slot.

**Implementation Steps:**
1. Initialize a fixed-size array where index for a given key is determined using `key % size`.
2. In case of a collision, use linear probing (examine next slot, `index = (current_index + 1) % size`) until an empty slot is found for `put`, or the key is found for `get` or `remove`.
3. For removal, one potential solution is to use a sentinel value indicating a removed spot (often `None`).

**Code:**
```python
class MyHashMap:
    def __init__(self):
        # Setting a large size to reduce collision
        self.size = 10000
        # Initialize the hash table with -1 indicating the slot is empty
        self.table = [-1] * self.size

    def _hash(self, key):
        return key % self.size

    def put(self, key: int, value: int) -> None:
        index = self._hash(key)
        # Linear probing in case of collision
        while self.table[index] != -1 and self.table[index][0] != key:
            index = (index + 1) % self.size
        self.table[index] = (key, value)

    def get(self, key: int) -> int:
        index = self._hash(key)
        # Search for the key using linear probing
        while self.table[index] != -1:
            if self.table[index][0] == key:
                return self.table[index][1]
            index = (index + 1) % self.size
        return -1

    def remove(self, key: int) -> None:
        index = self._hash(key)
        # Find the key to remove with linear probing
        while self.table[index] != -1:
            if self.table[index][0] == key:
                # Mark as deleted; handle with a sentinel or restructuring
                self.table[index] = (-1, -1)
                return
            index = (index + 1) % self.size
```

**Complexity Analysis:**
- Time Complexity: 
  - `put`: O(1) on average, but O(N) in the worst case due to clustering.
  - `get`: O(1) on average, but O(N) worst-case for probing across occupied slots.
  - `remove`: O(1) on average, but O(N) worst-case similar to get.
- Space Complexity: O(K) where K is the size of the hash table.

