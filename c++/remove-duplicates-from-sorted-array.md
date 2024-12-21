# [Leetcode 26: Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two-Pointer Technique](#approach-2-two-pointer-technique)

---

## Approach 1: Brute Force

**Intuition:**

In this approach, we use additional space to store the result, check each element, and append it to the new list if not already present. This naive approach doesn't take advantage of the sorted nature of the array and uses additional space.

**Pseudo approach:**

1. Iterate through the array, check each element.
2. Use another list to store unique elements.
3. If an element is not in the new list, append it.

**Code:**

```cpp
#include <vector>
#include <set>

class Solution {
public:
    int removeDuplicates(std::vector<int>& nums) {
        if (nums.empty()) return 0;
        
        // Using a set to track unique elements
        std::set<int> uniqueElements;
        
        for (int num : nums) {
            // Insert element in a set
            uniqueElements.insert(num);
        }
        
        // Overwrite original array with elements from the set
        int index = 0;
        for (int element : uniqueElements) {
            nums[index++] = element;
        }
        
        return uniqueElements.size();
    }
};
```

**Time Complexity:** `O(n log n)` due to set operations in the worst-case scenario.

**Space Complexity:** `O(n)` for storing elements in the set.

---

## Approach 2: Two-Pointer Technique

**Intuition:**

Since the array is sorted, duplicates will be adjacent. We can maintain a write index to overwrite duplicates. Another read index will traverse the entire array and only increment the write index for unique elements.

**Steps:**

1. Use two pointers, `i` as the write index and `j` as the read index.
2. Iterate over the array with `j`, comparing `nums[j]` with `nums[i - 1]`.
3. If `nums[j]` is different, increment `i` and set `nums[i]` to `nums[j]`.
4. `i` will be the length of the array with unique elements.

**Code:**

```cpp
#include <vector>

class Solution {
public:
    int removeDuplicates(std::vector<int>& nums) {
        if (nums.empty()) return 0;
        
        // Initialize the write index
        int i = 0;
        
        // Start with chosen read index
        for (int j = 1; j < nums.size(); j++) {
            if (nums[j] != nums[i]) {  // If a different element is found
                // Move write index forward and write unique element
                i++;
                nums[i] = nums[j];
            }
        }
        
        // The number of unique elements
        return i + 1;
    }
};
```

**Time Complexity:** `O(n)` since we traverse the list once.

**Space Complexity:** `O(1)` as we solve it in place without using extra space. 

This solution leverages the sorted nature of the array and efficiently handles the removal of duplicates through in-place updates.

