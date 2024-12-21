# [Leetcode 380: Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Approaches
1. [Using Array and HashMap](#using-array-and-hashmap)

---

### 1. Using Array and HashMap

To achieve the desired operations with O(1) time complexity, we can leverage the combination of an Array and a HashMap (or a JavaScript object for constant time complexity operations).

**Intuition:**
- **Insert**: To insert a value, we can simply append it to the end of the array and record its index in a HashMap.
- **Remove**: To remove a value, find its index using the HashMap. Swap this element with the last element of the array, update the HashMap for the swapped element, then pop the last element from the array. This maintains the array size without leaving gaps.
- **GetRandom**: Select a random index and return the element at that index.

### Code Implementation

```typescript
class RandomizedSet {
    private map: Map<number, number>;
    private list: number[];

    constructor() {
        this.map = new Map<number, number>(); // Map to store value -> index
        this.list = []; // Array to store values
    }

    insert(val: number): boolean {
        // If value already exists in the map, return false
        if (this.map.has(val)) return false;
        
        // Otherwise, add it to the map and list
        this.map.set(val, this.list.length); // Store value with its current index
        this.list.push(val); // Append to array
        return true;
    }

    remove(val: number): boolean {
        // If value does not exist, return false
        if (!this.map.has(val)) return false;
        
        const index = this.map.get(val)!; // Get index of the value
        const lastElement = this.list[this.list.length - 1]; // Last element in list

        // Swap the current element with the last element
        this.list[index] = lastElement;
        this.map.set(lastElement, index); // Update the index of the swapped element in the map

        // Remove the last element
        this.list.pop();
        this.map.delete(val); // Remove the element from the map
        return true;
    }

    getRandom(): number {
        // Generate a random index
        const randomIndex = Math.floor(Math.random() * this.list.length);
        return this.list[randomIndex]; // Return the element at the random index
    }
}
```

#### Time Complexity
- `Insert`: O(1) on average, as both the `push` to an array and `set` in a map are O(1) operations.
- `Remove`: O(1). We always swap and remove the last element in constant time.
- `GetRandom`: O(1). Random access in the array is constant.

#### Space Complexity
- O(n) where n is the number of elements in the data structure. This includes space for storing elements in the array and in the map.

