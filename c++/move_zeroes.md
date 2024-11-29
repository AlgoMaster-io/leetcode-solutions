# [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

## Approach 1: Brute Force

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        // Loop through the array
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == 0) {
                // Find the next non-zero element and swap
                for (int j = i + 1; j < nums.size(); j++) {
                    if (nums[j] != 0) {
                        // Swap the elements
                        int temp = nums[i];
                        nums[i] = nums[j];
                        nums[j] = temp;
                        break;
                    }
                }
            }
        }
    }
};
```

## Approach 2: Two-Pointer Technique

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int lastNonZeroFoundAt = 0; // Index to place the next non-zero element

        // Move all non-zero elements to the front of the array
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != 0) {
                nums[lastNonZeroFoundAt++] = nums[i];
            }
        }

        // Fill the remaining positions with zeros
        for (int i = lastNonZeroFoundAt; i < nums.size(); i++) {
            nums[i] = 0;
        }
    }
};
```

## Approach 3: Optimal Two-Pointer with Swapping

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int left = 0; // Pointer to track the position of the next non-zero element

        // Iterate through the array
        for (int right = 0; right < nums.size(); right++) {
            if (nums[right] != 0) {
                // Swap the non-zero element with the left pointer
                int temp = nums[left];
                nums[left] = nums[right];
                nums[right] = temp;
                left++; // Increment the left pointer
            }
        }
    }
};
```

