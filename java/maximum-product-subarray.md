# [Leetcode 152: Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Solutions
- [Solution 1: Brute Force](#solution-1-brute-force)
- [Solution 2: Optimized Dynamic Programming](#solution-2-optimized-dynamic-programming)

### Solution 1: Brute Force

**Intuition:**

The brute force approach is to generate all possible subarrays of the given array and calculate their products. Among all these products, we find the maximum product.

**Approach:**
1. Use a nested loop to consider every subarray from `i` to `j`.
2. For each subarray, compute the product.
3. Track the maximum product encountered.

**Code:**

```java
public int maxProductBruteForce(int[] nums) {
    // Initialize a variable to store the maximum product found
    int maxProduct = Integer.MIN_VALUE;
    
    // Iterate over every possible starting index of a subarray
    for (int i = 0; i < nums.length; i++) {
        int product = 1;
        // Calculate product for every subarray starting at index i
        for (int j = i; j < nums.length; j++) {
            product *= nums[j]; // Calculate product
            maxProduct = Math.max(maxProduct, product); // Update maximum product if needed
        }
    }
    return maxProduct;
}
```

**Time Complexity:** O(n^2) — Two nested loops to evaluate every possible subarray.

**Space Complexity:** O(1) — Constant space used.

### Solution 2: Optimized Dynamic Programming

**Intuition:**

The product of elements in subarrays can be disrupted by negative numbers and zeros. A negative number turns a positive product into negative and vice versa, while zero resets it. Thus, we need to track both the maximum and minimum product subarrays because a negative product might become the largest if multiplied by another negative number.

Instead of recalculating products for every possible subarray, we maintain two potential products:
- `maxProductSoFar`: Maximum product subarray ending at the current position.
- `minProductSoFar`: Minimum product subarray ending at the current position (to handle the case of two negative numbers resulting in a positive product).

**Approach:**
1. Iterate through the array, maintaining both `maxProductSoFar` and `minProductSoFar`.
2. If a negative number is encountered, swap `maxProductSoFar` and `minProductSoFar`.
3. Update the `maxProductSoFar` and `minProductSoFar` at each step.
4. Track the global maximum product.

**Code:**

```java
public int maxProduct(int[] nums) {
    // Edge case: if the array is empty
    if (nums == null || nums.length == 0) {
        return 0;
    }
    
    // Initialize the maximum and minimum product so far with the first element
    int maxProductSoFar = nums[0];
    int minProductSoFar = nums[0];
    // Result variable to store the final answer
    int maxProductResult = nums[0];
    
    // Iterate over the rest of the array from the second element
    for (int i = 1; i < nums.length; i++) {
        int currentNum = nums[i];
        // Temporarily store the maxProductSoFar for swapping if needed
        int tempMax = maxProductSoFar;
        
        // Update maxProductSoFar based on the current number and compared products
        maxProductSoFar = Math.max(currentNum, Math.max(maxProductSoFar * currentNum, minProductSoFar * currentNum));
        
        // Update minProductSoFar similarly to handle mixed signs
        minProductSoFar = Math.min(currentNum, Math.min(tempMax * currentNum, minProductSoFar * currentNum));
        
        // Update the result with the maximum of potentially new maxProductSoFar
        maxProductResult = Math.max(maxProductResult, maxProductSoFar);
    }
    return maxProductResult;
}
```

**Time Complexity:** O(n) — Iterate over the array once.

**Space Complexity:** O(1) — Only a constant amount of extra space is used to store intermediate results.

