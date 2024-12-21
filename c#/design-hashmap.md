# [Leetcode 706: Design HashMap](https://leetcode.com/problems/design-hashmap/)

## Approaches:
- [Approach 1: Array with Sequential Chaining](#approach-1-array-with-sequential-chaining)
- [Approach 2: Better Handling with Linked Lists](#approach-2-better-handling-with-linked-lists)
- [Approach 3: Optimized HashMap with Linked Lists and Improved Hash Function](#approach-3-optimized-hashmap-with-linked-lists-and-improved-hash-function)

---

## Approach 1: Array with Sequential Chaining

### Intuition:
The simplest way to design a hash map is to utilize an array to store our values. The challenge is to handle collisions when two keys hash to the same index. An easy approach to handle collisions is using sequential chaining where each bucket in our array holds a list of entries.

### Solution:
- Use an array of a fixed size, assuming a limited number of possible inputs.
- Each index contains a list of key-value pairs to handle collisions.
- This solution does not consider deletion of key and possible memory waste due to unused space in fixed array.

### Time Complexity:
- `Put`: O(N) in the worst case, where N is the number of keys in the chain for a particular index.
- `Get`: O(N) in the worst case.
- `Remove`: O(N) in the worst case.

### Space Complexity:
- O(M + N), where M is the size of the array and N is the number of inserted elements (due to chaining).

```csharp
public class MyHashMap {
    private const int size = 1000;
    private List<(int, int)>[] map;

    public MyHashMap() {
        map = new List<(int, int)>[size];
    }

    private int GetHash(int key) {
        return key % size;
    }
    
    public void Put(int key, int value) {
        int hash = GetHash(key);
        if (map[hash] == null) {
            map[hash] = new List<(int, int)>();
        }
        List<(int, int)> chain = map[hash];

        for (int i = 0; i < chain.Count; i++) {
            if (chain[i].Item1 == key) {
                chain[i] = (key, value);
                return;
            }
        }
        chain.Add((key, value));
    }
    
    public int Get(int key) {
        int hash = GetHash(key);
        List<(int, int)> chain = map[hash];
        if (chain == null) return -1;

        foreach (var pair in chain) {
            if (pair.Item1 == key) {
                return pair.Item2;
            }
        }
        return -1;
    }
    
    public void Remove(int key) {
        int hash = GetHash(key);
        List<(int, int)> chain = map[hash];
        if (chain == null) return;

        for (int i = 0; i < chain.Count; i++) {
            if (chain[i].Item1 == key) {
                chain.RemoveAt(i);
                break;
            }
        }
    }
}
```

---

## Approach 2: Better Handling with Linked Lists

### Intuition:
By using a linked list instead of a list for the chaining mechanism, we can efficiently manage our memory through dynamic allocation and avoid the overhead of list methods that rely on contiguous memory blocks.

### Solution:
- Each bucket in our array will hold a linked list of nodes.
- Each node contains a key-value pair.
- Nodes are dynamically allocated, avoiding issues with fixed-size lists.

### Time Complexity:
- `Put`: O(L) in the worst case, where L is the length of the linked list at a particular index.
- `Get`: O(L) in the worst case.
- `Remove`: O(L) in the worst case.

### Space Complexity:
- O(N + M), where N is the number of keys, due to the linked list nodes and M is the size of our array.

```csharp
public class MyHashMap {
    private const int size = 1000;
    private LinkedList<Entry>[] map;

    public class Entry {
        public int Key { get; set; }
        public int Value { get; set; }
        
        public Entry(int key, int value) {
            Key = key;
            Value = value;
        }
    }

    public MyHashMap() {
        map = new LinkedList<Entry>[size];
    }
    
    private int GetHash(int key) {
        return key % size;
    }
    
    public void Put(int key, int value) {
        int hash = GetHash(key);
        if (map[hash] == null) {
            map[hash] = new LinkedList<Entry>();
        }
        var entries = map[hash];

        foreach (var entry in entries) {
            if (entry.Key == key) {
                entry.Value = value;
                return;
            }
        }
        
        entries.AddLast(new Entry(key, value));
    }
    
    public int Get(int key) {
        int hash = GetHash(key);
        var entries = map[hash];
        if (entries == null) return -1;

        foreach (var entry in entries) {
            if (entry.Key == key) return entry.Value;
        }
        return -1;
    }
    
    public void Remove(int key) {
        int hash = GetHash(key);
        var entries = map[hash];
        if (entries == null) return;

        var node = entries.First;
        while(node != null) {
            if (node.Value.Key == key) {
                entries.Remove(node);
                break;
            }
            node = node.Next;
        }
    }
}
```

---

## Approach 3: Optimized HashMap with Linked Lists and Improved Hash Function

### Intuition:
We can improve both the efficiency of our hashmap and the handling of hash collisions by:
- Using a larger initial size for our array to reduce the number of collisions.
- Implementing a better hash function such as multiplicative hashing which helps spread out the entries more uniformly across the array.

### Solution:
- The bucket array size is increased and adjusted based on load factor.
- A prime multiplier in the hashing function further reduces the collisions.
- This approach retains dynamic allocation using linked-list nodes.

### Time Complexity:
- `Put`: O(1) on average due to reduced collisions.
- `Get`: O(1) on average.
- `Remove`: O(1) on average.

### Space Complexity:
- O(N + M), where N is the number of elements and M the dynamically maintained array size.

```csharp
public class MyHashMap {
    private const int initialCapacity = 2069;
    private LinkedList<Entry>[] map;

    public class Entry {
        public int Key { get; set; }
        public int Value { get; set; }
        
        public Entry(int key, int value) {
            Key = key;
            Value = value;
        }
    }

    public MyHashMap() {
        map = new LinkedList<Entry>[initialCapacity];
    }
    
    private int GetHash(int key) {
        int prime = 31;
        return (key * prime) % map.Length;
    }
    
    public void Put(int key, int value) {
        int hash = GetHash(key);
        if (map[hash] == null) {
            map[hash] = new LinkedList<Entry>();
        }
        var entries = map[hash];

        foreach (var entry in entries) {
            if (entry.Key == key) {
                entry.Value = value;
                return;
            }
        }
        
        entries.AddLast(new Entry(key, value));
    }
    
    public int Get(int key) {
        int hash = GetHash(key);
        var entries = map[hash];
        if (entries == null) return -1;

        foreach (var entry in entries) {
            if (entry.Key == key) return entry.Value;
        }
        return -1;
    }
    
    public void Remove(int key) {
        int hash = GetHash(key);
        var entries = map[hash];
        if (entries == null) return;

        var node = entries.First;
        while(node != null) {
            if (node.Value.Key == key) {
                entries.Remove(node);
                return;
            }
            node = node.Next;
        }
    }
}
```

---

