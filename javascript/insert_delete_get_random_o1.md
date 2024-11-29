# 380. [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Approach: HashMap with ArrayList

### Solution
```javascript
// Time Complexity: O(1) for insert, delete, and getRandom
// Space Complexity: O(n), where n is the number of elements in the data structure

class RandomizedSet {
    constructor() {
        this.valueToIndex = new Map();
        this.values = [];
    }

    insert(val) {
        if (this.valueToIndex.has(val)) {
            return false; // Element already exists
        }

        // Add the value to the list and its index to the map
        this.valueToIndex.set(val, this.values.length);
        this.values.push(val);
        return true;
    }

    remove(val) {
        if (!this.valueToIndex.has(val)) {
            return false; // Element does not exist
        }

        // Swap the element to be removed with the last element in the list
        const index = this.valueToIndex.get(val);
        const lastValue = this.values[this.values.length - 1];
        this.values[index] = lastValue;
        this.valueToIndex.set(lastValue, index);

        // Remove the last element
        this.values.pop();
        this.valueToIndex.delete(val);
        return true;
    }

    getRandom() {
        // Return a random element from the list
        return this.values[Math.floor(Math.random() * this.values.length)];
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * let obj = new RandomizedSet();
 * let param_1 = obj.insert(val);
 * let param_2 = obj.remove(val);
 * let param_3 = obj.getRandom();
 */
```

