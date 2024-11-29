# [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Approach 1: Left and Right Product Arrays

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <vector>

class Solution {
public:
    std::vector<int> productExceptSelf(std::vector<int>& nums) {
        int n = nums.size();
        std::vector<int> left(n);
        std::vector<int> right(n);
        std::vector<int> result(n);

        // Compute left products
        left[0] = 1;
        for (int i = 1; i < n; i++) {
            left[i] = left[i - 1] * nums[i - 1];
        }

        // Compute right products
        right[n - 1] = 1;
        for (int i = n - 2; i >= 0; i--) {
            right[i] = right[i + 1] * nums[i + 1];
        }

        // Compute result by multiplying left and right products
        for (int i = 0; i < n; i++) {
            result[i] = left[i] * right[i];
        }

        return result;
    }
};
```

## Approach 2: Optimized Space with Single Array

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1) (ignoring output array)
#include <vector>

class Solution {
public:
    std::vector<int> productExceptSelf(std::vector<int>& nums) {
        int n = nums.size();
        std::vector<int> result(n);

        // Compute left products directly in result
        result[0] = 1;
        for (int i = 1; i < n; i++) {
            result[i] = result[i - 1] * nums[i - 1];
        }

        // Compute right products and multiply with left products
        int right = 1;
        for (int i = n - 1; i >= 0; i--) {
            result[i] *= right;
            right *= nums[i];
        }

        return result;
    }
};
```

