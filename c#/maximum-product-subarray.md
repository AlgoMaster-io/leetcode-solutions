# [Leetcode 152: Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)
3. [Optimized Dynamic Programming Approach](#optimized-dynamic-programming-approach)

---

## Brute Force Approach

### Intuition
The naive way to solve this problem is to consider all possible subarrays and calculate their products. While this ensures we find the maximum product subarray, it is inefficient due to its complexity.

### Detailed Explanation
1. Iterate over each possible starting index of the subarray `i`.
2. Start another inner loop from the starting index `i` to the end of the array.
3. Compute the product of elements for each possible subarray starting at `i`.
4. Track the maximum product encountered.

### Time Complexity
- **O(n^2)**: We have double nested loops over `n` elements.
- **Space Complexity**: O(1): We only use a few extra variables.

### Code
```csharp
public class Solution {
    public int MaxProduct(int[] nums) {
        int maxProduct = int.MinValue;
        
        // Iterate over all possible starting points
        for (int i = 0; i < nums.Length; i++) {
            int product = 1;
            // Calculate product of subarray starting at i
            for (int j = i; j < nums.Length; j++) {
                product *= nums[j];
                // Update the max product found
                maxProduct = Math.Max(maxProduct, product);
            }
        }
        return maxProduct;
    }
}
```

---

## Dynamic Programming Approach

### Intuition
To improve on the brute force, we store the maximum and minimum products at each step. This approach is based on the observation that a negative number can result in a maximum product if we have previously encountered another negative number. Thus, for each element we maintain two variables `max_current` and `min_current`.

### Detailed Explanation
1. Initialize variables `max_global`, `max_current`, and `min_current` to the first element.
2. Starting from the second element, iterate through the array.
3. At each step, calculate the potential new `max_current` and `min_current` products due to the current element.
4. Update `max_current` and `min_current` using the max/min of:
   - The current element itself
   - The product of `max_current` and current element
   - The product of `min_current` and current element
5. Update `max_global` if `max_current` is greater.

### Time Complexity
- **O(n)**: We pass through each element once.
- **Space Complexity**: O(1): Only a few extra variables used.

### Code
```csharp
public class Solution {
    public int MaxProduct(int[] nums) {
        if (nums.Length == 0) return 0;
        
        int maxGlobal = nums[0];
        int maxCurrent = nums[0];
        int minCurrent = nums[0];
        
        // Iterate over each element from the second to the last
        for (int i = 1; i < nums.Length; i++) {
            int temp = maxCurrent;
            maxCurrent = Math.Max(nums[i], Math.Max(maxCurrent * nums[i], minCurrent * nums[i]));
            minCurrent = Math.Min(nums[i], Math.Min(temp * nums[i], minCurrent * nums[i]));
            // Update global maximum product found
            if (maxCurrent > maxGlobal) {
                maxGlobal = maxCurrent;
            }
        }
        
        return maxGlobal;
    }
}
```

---

## Optimized Dynamic Programming Approach

### Intuition
This approach refines the dynamic programming solution without changing the algorithm's core, ensuring clarity and encapsulated logic within loop iterations.

### Detailed Explanation
- As with the previous approach, iterate over the array maintaining `max_current` and `min_current`.
- Use concise variable updating that exploits tuple-like swapping for elegance and clarity.

### Time Complexity
- **O(n)**: Single pass through each element.
- **Space Complexity**: O(1): No extra space used other than few variables.

### Code
```csharp
public class Solution {
    public int MaxProduct(int[] nums) {
        int maxGlobal = nums[0], maxCurrent = nums[0], minCurrent = nums[0];

        for (int i = 1; i < nums.Length; i++) {
            int tempMax = maxCurrent;
            maxCurrent = Math.Max(nums[i], Math.Max(maxCurrent * nums[i], minCurrent * nums[i]));
            minCurrent = Math.Min(nums[i], Math.Min(tempMax * nums[i], minCurrent * nums[i]));
            maxGlobal = Math.Max(maxGlobal, maxCurrent);
        }

        return maxGlobal;
    }
}
```

Each approach builds upon the other, providing a pathway from a simple brute force to an elegant dynamic programming solution.

