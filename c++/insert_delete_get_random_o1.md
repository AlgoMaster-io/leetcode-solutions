# 380. [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Approach: HashMap with ArrayList

### Solution
```cpp
// Time Complexity: O(1) for insert, delete, and getRandom
// Space Complexity: O(n), where n is the number of elements in the data structure
#include <unordered_map>
#include <vector>
#include <cstdlib>

class RandomizedSet {
private:
    std::unordered_map<int, int> valueToIndex;
    std::vector<int> values;

public:
    RandomizedSet() {
        // No initialization needed as the data members are already default-constructed
    }

    bool insert(int val) {
        if (valueToIndex.find(val) != valueToIndex.end()) {
            return false; // Element already exists
        }

        // Add the value to the list and its index to the map
        valueToIndex[val] = values.size();
        values.push_back(val);
        return true;
    }

    bool remove(int val) {
        if (valueToIndex.find(val) == valueToIndex.end()) {
            return false; // Element does not exist
        }

        // Swap the element to be removed with the last element in the list
        int index = valueToIndex[val];
        int lastValue = values.back();
        values[index] = lastValue;
        valueToIndex[lastValue] = index;

        // Remove the last element
        values.pop_back();
        valueToIndex.erase(val);
        return true;
    }

    int getRandom() {
        // Return a random element from the list
        int randomIndex = rand() % values.size();
        return values[randomIndex];
    }
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

