# [Leetcode 380: Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Solutions

- [Solution 1: Using a HashMap and Array](#solution-1-using-a-hashmap-and-array)

### Solution 1: Using a HashMap and Array

**Intuition:**

To achieve average `O(1)` time complexity for insert, delete, and getRandom operations, we can combine the properties of an array with those of a hash map.

- **Array** will store the elements to allow `O(1)` access and `getRandom` operations.
- **Hash Map** will keep track of the indices of each element in the array, allowing for `O(1)` insert and delete operations.

We solve the removal problem efficiently by swapping the element to be deleted with the last element in the array and then removing the last element, which ensures the array’s stability without needing to do a full linear scan.

**Implementation Steps:**

1. **Initialization**: Create an array to store values and a hash map to store each value's index in the array.
2. **Insert**:
   - If the element is already in the hash map, do nothing.
   - Otherwise, append the element to the array and store its index in the hash map.
3. **Remove**:
   - If the element is not in the hash map, do nothing.
   - Otherwise, swap the element with the last element of the array (if they are different).
   - Update hash map entry for the swapped element to its new index.
   - Remove the element from both the hash map and the array.
4. **GetRandom**: Pick a random index from the array and return the element at that index.

**Code:**

```javascript
class RandomizedSet {
    constructor() {
        this.list = [];
        this.map = new Map(); // key: element, value: index in the list
    }
    
    insert(val) {
        if (this.map.has(val)) {
            return false; // Element already exists
        }
        // Append to the list
        this.map.set(val, this.list.length);
        this.list.push(val);
        return true;
    }
    
    remove(val) {
        if (!this.map.has(val)) {
            return false; // Element doesn't exist
        }
        // Remove element by swapping with the last element
        const lastElement = this.list[this.list.length - 1];
        const index = this.map.get(val);

        this.list[index] = lastElement; // Overwrite the element to be removed with the last one
        this.map.set(lastElement, index); // Update last element's index in map

        // Now remove the last element
        this.list.pop();
        this.map.delete(val);
        return true;
    }
    
    getRandom() {
        const randomIndex = Math.floor(Math.random() * this.list.length);
        return this.list[randomIndex];
    }
}
```

**Complexity Analysis:**

- **Time Complexity**: All operations—`insert`, `remove`, and `getRandom`—take, on average, `O(1)` time.
- **Space Complexity**: `O(n)`, where `n` is the number of elements stored. Both the array and map store `n` elements. 

This solution efficiently handles the requirements of the problem statement and is both clean and optimal.

