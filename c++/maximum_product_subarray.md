# [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int maxProduct = INT_MIN;

        // Iterate through all possible subarrays
        for (int i = 0; i < nums.size(); i++) {
            int currentProduct = 1;

            for (int j = i; j < nums.size(); j++) {
                currentProduct *= nums[j];
                maxProduct = max(maxProduct, currentProduct);
            }
        }

        return maxProduct;
    }
};
```

## Approach 2: Kadane's algorithm with Min/Max Tracking (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int maxProduct = nums[0];
        int currentMax = nums[0];
        int currentMin = nums[0];

        // Traverse the array
        for (int i = 1; i < nums.size(); i++) {
            int num = nums[i];

            // Swap currentMax and currentMin if num is negative
            if (num < 0) {
                swap(currentMax, currentMin);
            }

            // Update currentMax and currentMin
            currentMax = max(num, currentMax * num);
            currentMin = min(num, currentMin * num);

            // Update the global maximum product
            maxProduct = max(maxProduct, currentMax);
        }

        return maxProduct;
    }
};
```

