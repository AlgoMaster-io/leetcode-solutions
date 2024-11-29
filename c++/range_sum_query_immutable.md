# [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n) per query
// Space Complexity: O(1)
class NumArray {
private:
    std::vector<int> nums;

public:
    NumArray(std::vector<int>& nums) : nums(nums) {}

    int sumRange(int left, int right) {
        int sum = 0;

        // Calculate the sum for the range [left, right]
        for (int i = left; i <= right; i++) {
            sum += nums[i];
        }

        return sum;
    }
};
```

## Approach 2: Prefix Sum (Optimal)

### Solution
```cpp
// Time Complexity: O(1) per query, O(n) for initialization
// Space Complexity: O(n)
class NumArray {
private:
    std::vector<int> prefixSums;

public:
    NumArray(std::vector<int>& nums) {
        int n = nums.size();
        prefixSums.resize(n + 1, 0); // Initialize prefix sum array

        // Compute prefix sums
        for (int i = 0; i < n; i++) {
            prefixSums[i + 1] = prefixSums[i] + nums[i];
        }
    }

    int sumRange(int left, int right) {
        // Use the prefix sum array to calculate the range sum
        return prefixSums[right + 1] - prefixSums[left];
    }
};
```

