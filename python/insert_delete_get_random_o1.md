# 380. [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Approach: HashMap with List

### Solution
python
```python
# Time Complexity: O(1) for insert, delete, and getRandom
# Space Complexity: O(n), where n is the number of elements in the data structure
import random

class RandomizedSet:
    def __init__(self):
        self.value_to_index = {}
        self.values = []

    def insert(self, val: int) -> bool:
        if val in self.value_to_index:
            return False  # Element already exists

        # Add the value to the list and its index to the map
        self.value_to_index[val] = len(self.values)
        self.values.append(val)
        return True

    def remove(self, val: int) -> bool:
        if val not in self.value_to_index:
            return False  # Element does not exist

        # Swap the element to be removed with the last element in the list
        index = self.value_to_index[val]
        last_value = self.values[-1]
        self.values[index] = last_value
        self.value_to_index[last_value] = index

        # Remove the last element
        self.values.pop()
        del self.value_to_index[val]
        return True

    def getRandom(self) -> int:
        # Return a random element from the list
        return random.choice(self.values)

# Example usage:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```

