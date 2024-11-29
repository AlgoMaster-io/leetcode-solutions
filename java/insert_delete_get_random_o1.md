# 380. [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Approach: HashMap with ArrayList

### Solution
```java
// Time Complexity: O(1) for insert, delete, and getRandom
// Space Complexity: O(n), where n is the number of elements in the data structure
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Random;

class RandomizedSet {
    private HashMap<Integer, Integer> valueToIndex;
    private ArrayList<Integer> values;
    private Random random;

    public RandomizedSet() {
        valueToIndex = new HashMap<>();
        values = new ArrayList<>();
        random = new Random();
    }

    public boolean insert(int val) {
        if (valueToIndex.containsKey(val)) {
            return false; // Element already exists
        }

        // Add the value to the list and its index to the map
        valueToIndex.put(val, values.size());
        values.add(val);
        return true;
    }

    public boolean remove(int val) {
        if (!valueToIndex.containsKey(val)) {
            return false; // Element does not exist
        }

        // Swap the element to be removed with the last element in the list
        int index = valueToIndex.get(val);
        int lastValue = values.get(values.size() - 1);
        values.set(index, lastValue);
        valueToIndex.put(lastValue, index);

        // Remove the last element
        values.remove(values.size() - 1);
        valueToIndex.remove(val);
        return true;
    }

    public int getRandom() {
        // Return a random element from the list
        return values.get(random.nextInt(values.size()));
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```