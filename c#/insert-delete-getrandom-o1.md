# [Leetcode 380: Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## Solutions
- [Approach 1: Using Dictionary and List](#approach-1-using-dictionary-and-list)
- [Approach 2: Optimized with Randomness Using Efficient Data Structures](#approach-2-optimized-with-randomness-using-efficient-data-structures)

### Approach 1: Using Dictionary and List

#### Intuition

To achieve O(1) insertion, deletion, and random access, we can utilize a combination of a `List` and a `Dictionary`:

1. **List**: It maintains the elements and provides O(1) random access. It will store the actual values.
2. **Dictionary**: It maps values to their indices in the list. This gives us O(1) time complexity for finding the index of an element, which is crucial for deletion.

For insertion, we add the element to the end of the list and store its position in the dictionary.

For deletion, we swap the element to be removed with the last element in the list and then remove the last element. This keeps the list compact and allows efficient removal.

For getting a random element, we directly access a random index from the list, which is efficient.

#### Code

```csharp
using System;
using System.Collections.Generic;

public class RandomizedSet {
    private List<int> nums;
    private Dictionary<int, int> dict;
    private Random random;

    public RandomizedSet() {
        nums = new List<int>();
        dict = new Dictionary<int, int>();
        random = new Random();
    }

    public bool Insert(int val) {
        // If the value is already in the dictionary, return false (already present).
        if (dict.ContainsKey(val)) return false;
        
        // Add the value to the list.
        nums.Add(val);
        // Store the index of the value in the dictionary.
        dict[val] = nums.Count - 1;
        
        return true;
    }

    public bool Remove(int val) {
        // If the value is not in the dictionary, return false (not present).
        if (!dict.ContainsKey(val)) return false;
        
        // Obtain the index of the value in the list.
        int index = dict[val];
        // Obtain the last element in the list.
        int lastElement = nums[nums.Count - 1];
        
        // Swap the last element with the element to delete.
        nums[index] = lastElement;
        // Update the index of the last element in the dictionary.
        dict[lastElement] = index;
        
        // Remove the last element from the list.
        nums.RemoveAt(nums.Count - 1);
        // Remove the deleted element from the dictionary.
        dict.Remove(val);
        
        return true;
    }

    public int GetRandom() {
        // Generate a random index and return the element at that index.
        int randomIndex = random.Next(nums.Count);
        return nums[randomIndex];
    }
}
```

#### Complexity Analysis
- **Time Complexity**: 
  - Insert: O(1)
  - Delete: O(1)
  - GetRandom: O(1)
- **Space Complexity**: O(n), where n is the number of elements stored in the list and dictionary.

### Approach 2: Optimized with Randomness Using Efficient Data Structures

This approach already achieves the desired time complexities optimally. With the combination of a list for O(1) access and a dictionary for O(1) index retrieval and updates, there's no further optimization available in terms of fundamental operations within the constraints of the problem.

The above approach effectively solves the problem while meeting all constraints by utilizing efficient data structures.

