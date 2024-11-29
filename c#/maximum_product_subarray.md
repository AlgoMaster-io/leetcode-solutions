# [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int MaxProduct(int[] nums) {
        int maxProduct = int.MinValue;

        // Iterate through all possible subarrays
        for (int i = 0; i < nums.Length; i++) {
            int currentProduct = 1;

            for (int j = i; j < nums.Length; j++) {
                currentProduct *= nums[j];
                maxProduct = Math.Max(maxProduct, currentProduct);
            }
        }

        return maxProduct;
    }
}
```

## Approach 2: Kadane's algorithm with Min/Max Tracking (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int MaxProduct(int[] nums) {
        int maxProduct = nums[0];
        int currentMax = nums[0];
        int currentMin = nums[0];

        // Traverse the array
        for (int i = 1; i < nums.Length; i++) {
            int num = nums[i];

            // Swap currentMax and currentMin if num is negative
            if (num < 0) {
                int temp = currentMax;
                currentMax = currentMin;
                currentMin = temp;
            }

            // Update currentMax and currentMin
            currentMax = Math.Max(num, currentMax * num);
            currentMin = Math.Min(num, currentMin * num);

            // Update the global maximum product
            maxProduct = Math.Max(maxProduct, currentMax);
        }

        return maxProduct;
    }
}
```

