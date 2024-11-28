# [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int maxProduct(int[] nums) {
        int maxProduct = Integer.MIN_VALUE;

        // Iterate through all possible subarrays
        for (int i = 0; i < nums.length; i++) {
            int currentProduct = 1;

            for (int j = i; j < nums.length; j++) {
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
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int maxProduct(int[] nums) {
        int maxProduct = nums[0];
        int currentMax = nums[0];
        int currentMin = nums[0];

        // Traverse the array
        for (int i = 1; i < nums.length; i++) {
            int num = nums[i];

            // Swap currentMax and currentMin if num is negative
            if (num < 0) {
                int temp = currentMax;
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