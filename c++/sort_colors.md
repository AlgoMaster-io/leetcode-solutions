# [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

## Approach 1: Two-Pass Counting Sort

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int count[3] = {0};

        // Count the occurrences of 0, 1, and 2
        for (int num : nums) {
            count[num]++;
        }

        // Overwrite the array based on the counts
        int index = 0;
        for (int i = 0; i < 3; i++) {
            while (count[i] > 0) {
                nums[index++] = i;
                count[i]--;
            }
        }
    }
};
```

## Approach 2: One-Pass (Dutch National Flag Algorithm)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int low = 0, mid = 0, high = nums.size() - 1;

        while (mid <= high) {
            if (nums[mid] == 0) {
                // Swap nums[low] and nums[mid] and move pointers
                swap(nums[low++], nums[mid++]);
            } else if (nums[mid] == 1) {
                // Just move the mid pointer
                mid++;
            } else {
                // Swap nums[mid] and nums[high] and move high pointer
                swap(nums[mid], nums[high--]);
            }
        }
    }

private:
    void swap(int& a, int& b) {
        int temp = a;
        a = b;
        b = temp;
    }
};
```

