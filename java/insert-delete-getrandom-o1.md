# [Leetcode 380: Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Approaches
- [Approach 1: List and HashSet](#approach-1-list-and-hashset)
- [Approach 2: HashMap and ArrayList](#approach-2-hashmap-and-arraylist)

### Approach 1: List and HashSet

#### Intuition
We can use a combination of a list and a set to achieve the operations required by the problem. The list allows us to efficiently access and remove elements with the help of an index, while the set ensures that we don't insert duplicate elements, owing to its unique property. However, removing an element from the middle of the list is an O(n) operation, which doesn't meet the optimal constraint, but it is a stepping stone towards understanding the problem.

#### Implementation

```java
import java.util.*;

class RandomizedSet {
    List<Integer> list;
    Set<Integer> set;

    /** Initialize your data structure here. */
    public RandomizedSet() {
        list = new ArrayList<>();
        set = new HashSet<>();
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    public boolean insert(int val) {
        // Check if value already exists
        if (set.contains(val)) return false;
        // Add to list and set
        set.add(val);
        list.add(val);
        return true;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    public boolean remove(int val) {
        // Check if value exists
        if (!set.contains(val)) return false;
        // Remove from set and list
        set.remove(val);
        list.remove((Integer) val);  // Explicit cast to object to call remove(object)
        return true;
    }
    
    /** Get a random element from the set. */
    public int getRandom() {
        // Return random element from list
        return list.get((int) (Math.random() * list.size()));
    }
}
```

**Complexity Analysis**

- **Time Complexity:** `O(n)` for `remove()`, and `O(1)` for `insert()` and `getRandom()`.
- **Space Complexity:** `O(n)` for storing `n` elements in the list and set.

---

### Approach 2: HashMap and ArrayList

#### Intuition
To achieve `O(1)` on all operations, we can use a combination of a HashMap to store elements and their indices, and an ArrayList to store the elements. The HashMap gives us `O(1)` access and removal time when handling key-value pairs, while the ArrayList provides `O(1)` time for random access. The trick to efficiently remove an element from the list is to swap the element to remove with the last element, then remove the last element. This approach retains the order only up to the end where the swap happens, which is acceptable since we only need `getRandom` to return any element.

#### Implementation

```java
import java.util.*;

class RandomizedSet {
    private ArrayList<Integer> list;
    private HashMap<Integer, Integer> map;

    /** Initialize your data structure here. */
    public RandomizedSet() {
        list = new ArrayList<>();
        map = new HashMap<>();
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    public boolean insert(int val) {
        if (map.containsKey(val)) {
            return false;
        }
        // Add to map and list
        map.put(val, list.size());
        list.add(val);
        return true;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    public boolean remove(int val) {
        if (!map.containsKey(val)) {
            return false;
        }
        // Get index from map
        int index = map.get(val);
        int lastElement = list.get(list.size() - 1);
        
        // Swap element with the last element
        list.set(index, lastElement);
        map.put(lastElement, index);
        
        // Remove last element
        list.remove(list.size() - 1);
        map.remove(val);
        return true;
    }
    
    /** Get a random element from the set. */
    public int getRandom() {
        // Choose a random index and return the element
        return list.get((int) (Math.random() * list.size()));
    }
}
```

**Complexity Analysis**

- **Time Complexity:** `O(1)` for all operations (`insert()`, `remove()`, `getRandom()`).
- **Space Complexity:** `O(n)` for storing `n` elements in the map and list.

This approach meets the problem's constraints efficiently with `O(1)` time complexity for all operations.

