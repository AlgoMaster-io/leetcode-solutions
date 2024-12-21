# Leetcode Problem: Product of Array Except Self

[Leetcode Problem 238: Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Approaches:
1. [Basic Approach: Using Two Arrays](#basic-approach-using-two-arrays)
2. [Optimized Approach: Using Single Array and Prefix Products](#optimized-approach-using-single-array-and-prefix-products)
3. [Most Optimal Approach: Without Extra Space](#most-optimal-approach-without-extra-space)

---

### Basic Approach: Using Two Arrays

#### Intuition:
In this basic approach, we use two arrays to store the prefix and suffix products for each index. The prefix product at an index `i` is the product of all elements to the left, and the suffix product is the product of all elements to the right. The result for each index is then simply the product of the corresponding prefix and suffix products.

#### Code:
```csharp
public class Solution {
    public int[] ProductExceptSelf(int[] nums) {
        int n = nums.Length;
        int[] result = new int[n];
        int[] prefixProducts = new int[n];
        int[] suffixProducts = new int[n];
        
        // Construct the prefix products
        prefixProducts[0] = 1; // Start with 1 as there's no product on the left of the first element
        for (int i = 1; i < n; i++) {
            prefixProducts[i] = prefixProducts[i - 1] * nums[i - 1];
        }
        
        // Construct the suffix products
        suffixProducts[n - 1] = 1; // Start with 1 as there's no product on the right of the last element
        for (int i = n - 2; i >= 0; i--) {
            suffixProducts[i] = suffixProducts[i + 1] * nums[i + 1];
        }
        
        // Calculate result by multiplying prefix and suffix products
        for (int i = 0; i < n; i++) {
            result[i] = prefixProducts[i] * suffixProducts[i];
        }
        
        return result;
    }
}
```

#### Complexity:
- **Time Complexity:** O(n), where n is the length of the input array `nums`.
- **Space Complexity:** O(n), due to the use of prefix and suffix arrays.

---

### Optimized Approach: Using Single Array and Prefix Products

#### Intuition:
Instead of using two separate arrays for storing prefix and suffix products, we will use the result array itself to store the left prefix products. After that, we will traverse from the right to incorporate the suffix product dynamically while computing the final result array.

#### Code:
```csharp
public class Solution {
    public int[] ProductExceptSelf(int[] nums) {
        int n = nums.Length;
        int[] result = new int[n];
        
        // Initializing the first element of result with 1 as it has no left to multiply
        result[0] = 1;
        
        // Forward pass to fill the result with prefix products
        for (int i = 1; i < n; i++) {
            result[i] = result[i - 1] * nums[i - 1];
        }
        
        // Backward pass to incorporate the suffix products
        int suffixProduct = 1;  // Suffix product for last element initialization
        for (int i = n - 1; i >= 0; i--) {
            result[i] *= suffixProduct;
            suffixProduct *= nums[i]; // Update suffixProduct for the next iteration
        }
        
        return result;
    }
}
```

#### Complexity:
- **Time Complexity:** O(n)
- **Space Complexity:** O(1) beyond the output array, as we make updates in place.

---

### Most Optimal Approach: Without Extra Space

#### Intuition:
The previous solution is already optimized in terms of space, using only O(1) additional space beyond the output array. However, this approach emphasizes clarity in understanding that we are effectively using the result array for both prefix and suffix multiplication operations directly.

#### Code: 
The implementation remains the same as the optimized approach since it already achieves the optimal space complexity:
```csharp
public class Solution {
    public int[] ProductExceptSelf(int[] nums) {
        int n = nums.Length;
        int[] result = new int[n];
        
        // Initialize the first position with 1
        result[0] = 1;
        
        // Fill in prefix products
        for (int i = 1; i < n; i++) {
            result[i] = result[i - 1] * nums[i - 1];
        }
        
        // Multiply with suffix products
        int suffixProduct = 1;
        for (int i = n - 1; i >= 0; i--) {
            result[i] *= suffixProduct;
            suffixProduct *= nums[i];
        }
        
        return result;
    }
}
```

#### Complexity:
- **Time Complexity:** O(n)
- **Space Complexity:** O(1) beyond the output array.

---

