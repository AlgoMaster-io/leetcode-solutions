# [Leetcode 380: Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Approaches:
1. [Naive Approach](#naive-approach)
2. [Optimal Approach using Hashmap and List](#optimal-approach-using-hashmap-and-list)

### Naive Approach

#### Intuition:
The problem requires exact O(1) complexity for insert, delete, and get random operations. The naive approach to attempt might involve using either a list or a set that supports some of these operations with O(1) time, but not all together:

- **Insert:** Use an unordered set to store unique elements.
- **Delete:** Use the unordered set's `erase` method.
- **GetRandom:** Convert the set to a list and use a random index to get the element.

However, note that while insertion and deletion may be O(1), get random would not be, since we have to convert the set into a list.

#### Code:

```cpp
// This approach does not meet the problem's requirements as getRandom is not O(1)
```

#### Time and Space Complexity:
- **Time Complexity:** 
  - Insert: O(1)
  - Delete: O(1)
  - GetRandom: O(n)
- **Space Complexity:** O(n)

This approach is ineffective for the problem constraints since `getRandom` is not O(1).

### Optimal Approach using Hashmap and List

#### Intuition:
The combination of a list and a hash map can fulfill the O(1) complexity requirement for all operations:
- Use a vector to store the elements, allowing O(1) access to elements by index.
- Use an unordered map to store the position/index of each element in the list to support O(1) deletion.

**How the operations work:**
- **Insert(int val):** 
  - Only insert if the element does not exist.
  - Store the element at the end of the list and record its index in the map.
- **Remove(int val):** 
  - Check if the element exists using the map.
  - Swap the element with the last element in the list for O(1) removal, then update the affected index in the map, and pop the last element from the vector.
- **GetRandom():**
  - Use `rand()` to select a random index and return the element at that index from the list/vector.

#### Code:

```cpp
#include <unordered_map>
#include <vector>
#include <cstdlib>

class RandomizedSet {
private:
    std::unordered_map<int, int> valToIndex; // Maps value to its index in the vector.
    std::vector<int> elements; // Stores all elements.

public:
    RandomizedSet() {}

    bool insert(int val) {
        // If value already exists, return false.
        if (valToIndex.find(val) != valToIndex.end()) {
            return false;
        }
        // Add the value and index it correctly.
        elements.push_back(val);
        valToIndex[val] = elements.size() - 1;
        return true;
    }

    bool remove(int val) {
        // If value does not exist, return false.
        if (valToIndex.find(val) == valToIndex.end()) {
            return false;
        }
        
        // Find index of the element to remove.
        int indexToRemove = valToIndex[val];
        // Swap it with the last element.
        int lastElement = elements.back();
        elements[indexToRemove] = lastElement;
        valToIndex[lastElement] = indexToRemove;

        // Remove the element.
        elements.pop_back();
        valToIndex.erase(val);
        return true;
    }

    int getRandom() {
        // Return a random element from the list.
        int randomIndex = rand() % elements.size();
        return elements[randomIndex];
    }
};
```

#### Time and Space Complexity:
- **Time Complexity:** 
  - Insert: O(1)
  - Delete: O(1)
  - GetRandom: O(1)
- **Space Complexity:** O(n), where n is the number of elements stored. 

This approach meets all constraints of the problem and provides efficient operations.

