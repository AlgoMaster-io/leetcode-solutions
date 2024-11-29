# [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Approach 1: Merge Sort (Divide and Conquer)

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <vector>
#include <algorithm>

class Solution {
public:
    int reversePairs(std::vector<int>& nums) {
        if (nums.size() < 2) {
            return 0;
        }
        return mergeSort(nums, 0, nums.size() - 1);
    }

private:
    int mergeSort(std::vector<int>& nums, int left, int right) {
        if (left >= right) {
            return 0;
        }

        int mid = left + (right - left) / 2;

        // Count reverse pairs in left and right halves, and across them
        int count = mergeSort(nums, left, mid) + mergeSort(nums, mid + 1, right);

        // Count cross reverse pairs
        int j = mid + 1;
        for (int i = left; i <= mid; ++i) {
            while (j <= right && static_cast<long>(nums[i]) > 2L * nums[j]) {
                ++j;
            }
            count += (j - mid - 1);
        }

        // Merge the two halves
        merge(nums, left, mid, right);

        return count;
    }

    void merge(std::vector<int>& nums, int left, int mid, int right) {
        std::vector<int> temp(right - left + 1);
        int i = left, j = mid + 1, k = 0;

        while (i <= mid && j <= right) {
            if (nums[i] <= nums[j]) {
                temp[k++] = nums[i++];
            } else {
                temp[k++] = nums[j++];
            }
        }

        while (i <= mid) {
            temp[k++] = nums[i++];
        }

        while (j <= right) {
            temp[k++] = nums[j++];
        }

        std::copy(temp.begin(), temp.end(), nums.begin() + left);
    }
};
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
```cpp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
#include <vector>
#include <set>
#include <map>

class Solution {
public:
    int reversePairs(std::vector<int>& nums) {
        // Coordinate compression
        std::set<long> set;
        for (int num : nums) {
            set.insert(static_cast<long>(num));
            set.insert(static_cast<long>(num) * 2);
        }

        std::map<long, int> map;
        int rank = 1;
        for (long num : set) {
            map[num] = rank++;
        }

        // Binary Indexed Tree
        std::vector<int> bit(rank);
        int count = 0;

        for (int i = nums.size() - 1; i >= 0; --i) {
            // Count elements smaller than nums[i]
            count += query(bit, map[static_cast<long>(nums[i])] - 1);

            // Add nums[i] * 2 to the BIT
            update(bit, map[static_cast<long>(nums[i]) * 2], 1);
        }

        return count;
    }

private:
    void update(std::vector<int>& bit, int index, int delta) {
        while (index < bit.size()) {
            bit[index] += delta;
            index += index & -index;
        }
    }

    int query(const std::vector<int>& bit, int index) {
        int sum = 0;
        while (index > 0) {
            sum += bit[index];
            index -= index & -index;
        }
        return sum;
    }
};
```

