# [Leetcode Problem 41: First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [HashSet Approach](#hashset-approach)
3. [Cyclic Sort Approach](#cyclic-sort-approach)

---

## Brute Force Approach

### Intuition
The simplest approach is to identify the smallest missing positive by starting from 1 and incrementally checking for its presence in the array. Although straightforward, this approach may not perform efficiently on larger arrays.

### Approach
1. For every number starting from 1, check if it exists in the array.
2. If it doesn't exist, return it as the first missing positive.

### C++ Code
```cpp
int firstMissingPositive(vector<int>& nums) {
    int n = nums.size();
    
    // Start checking from 1 for each positive integer
    for (int i = 1; i <= n + 1; ++i) {
        bool found = false;
        
        // Check if current integer i is present
        for (int num : nums) {
            if (i == num) {
                found = true;
                break;
            }
        }
        
        // If not found, it is our result
        if (!found) {
            return i;
        }
    }
    return 1; // theoretically unreachable if the input is valid
}
```

### Complexity
- **Time Complexity**: O(n^2), where n is the length of the array, due to nested iteration.
- **Space Complexity**: O(1)

---

## HashSet Approach

### Intuition
Using a hash set allows us to easily check the presence of each number while maintaining a more efficient lookup time compared to a brute force approach.

### Approach
1. Insert all numbers into a hash set.
2. Starting from 1, check for each consecutive integer's presence in the hash set.
3. The first integer not found in the hash set is the missing positive integer.

### C++ Code
```cpp
#include <unordered_set>

int firstMissingPositive(vector<int>& nums) {
    unordered_set<int> numSet(nums.begin(), nums.end());
    
    // Check each integer starting from 1
    for (int i = 1; i <= nums.size() + 1; ++i) {
        // Return the first integer not found in set
        if (numSet.find(i) == numSet.end()) {
            return i;
        }
    }
    return 1; // theoretically unreachable if the input is valid
}
```

### Complexity
- **Time Complexity**: O(n), only iterating and checking set members.
- **Space Complexity**: O(n), due to usage of an additional hash set.

---

## Cyclic Sort Approach

### Intuition
The optimal solution leverages the fact that we are seeking the first missing positive. By rearranging numbers so that each positive number n is placed at index n-1, we can easily identify the first missing positive as the index where the rule breaks.

### Approach
1. Iterate over the array, attempting to place each number n where n belongs (at index n-1).
2. Swap numbers to rearrange them if they are within the range of indices.
3. After sorting, the first place i where nums[i] != i + 1 provides the missing positive integer.

### C++ Code
```cpp
int firstMissingPositive(vector<int>& nums) {
    int n = nums.size();
    
    for (int i = 0; i < n; ++i) {
        // Place each number in its 'rightful' place if it's within range 1 to n
        while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
            swap(nums[i], nums[nums[i] - 1]);
        }
    }
    
    // Identify the first missing positive
    for (int i = 0; i < n; ++i) {
        if (nums[i] != i + 1) {
            return i + 1;
        }
    }
    
    return n + 1; // if all are in position, next integer beyond array length
}
```

### Complexity
- **Time Complexity**: O(n), since each number is placed at its position at most once.
- **Space Complexity**: O(1), in-place rearrangement.

