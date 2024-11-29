# [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
    maxProduct(nums: number[]): number {
        let maxProduct = Number.MIN_SAFE_INTEGER;

        // Iterate through all possible subarrays
        for (let i = 0; i < nums.length; i++) {
            let currentProduct = 1;

            for (let j = i; j < nums.length; j++) {
                currentProduct *= nums[j];
                maxProduct = Math.max(maxProduct, currentProduct);
            }
        }

        return maxProduct;
    }
}
```

## Approach 2: Kadane's algorithm with Min/Max Tracking (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    maxProduct(nums: number[]): number {
        let maxProduct = nums[0];
        let currentMax = nums[0];
        let currentMin = nums[0];

        // Traverse the array
        for (let i = 1; i < nums.length; i++) {
            const num = nums[i];

            // Swap currentMax and currentMin if num is negative
            if (num < 0) {
                const temp = currentMax;
                currentMax = currentMin;
                currentMin = temp;
            }

            // Update currentMax and currentMin
            currentMax = Math.max(num, currentMax * num);
            currentMin = Math.min(num, currentMin * num);

            // Update the global maximum product
            maxProduct = Math.max(maxProduct, currentMax);
        }

        return maxProduct;
    }
}
```

