# 380. [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Approach: Dictionary with List

### Solution
csharp
```csharp
// Time Complexity: O(1) for insert, delete, and getRandom
// Space Complexity: O(n), where n is the number of elements in the data structure
using System;
using System.Collections.Generic;

public class RandomizedSet {
    private Dictionary<int, int> valueToIndex;
    private List<int> values;
    private Random random;

    public RandomizedSet() {
        valueToIndex = new Dictionary<int, int>();
        values = new List<int>();
        random = new Random();
    }

    public bool Insert(int val) {
        if (valueToIndex.ContainsKey(val)) {
            return false; // Element already exists
        }

        // Add the value to the list and its index to the map
        valueToIndex[val] = values.Count;
        values.Add(val);
        return true;
    }

    public bool Remove(int val) {
        if (!valueToIndex.ContainsKey(val)) {
            return false; // Element does not exist
        }

        // Swap the element to be removed with the last element in the list
        int index = valueToIndex[val];
        int lastValue = values[values.Count - 1];
        values[index] = lastValue;
        valueToIndex[lastValue] = index;

        // Remove the last element
        values.RemoveAt(values.Count - 1);
        valueToIndex.Remove(val);
        return true;
    }

    public int GetRandom() {
        // Return a random element from the list
        return values[random.Next(values.Count)];
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * bool param_1 = obj.Insert(val);
 * bool param_2 = obj.Remove(val);
 * int param_3 = obj.GetRandom();
 */
```

