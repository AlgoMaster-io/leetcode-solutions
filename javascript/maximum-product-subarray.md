# [Leetcode 152: Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Approaches:

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)
- [Approach 3: Optimized Dynamic Programming (Two-pass with Cache)](#approach-3-optimized-dynamic-programming-two-pass-with-cache)

### Approach 1: Brute Force

In this approach, we'll calculate the product of every possible subarray and keep track of the maximum product found.

1. Initialize `maxProduct` to store the maximum product found so far.
2. Use two nested loops. The outer loop starts from each element, and the inner loop calculates the product of every subarray starting from the outer loop element.
3. Update the `maxProduct` if the current product is greater.

#### Code:

```javascript
function maxProduct(nums) {
    let maxProduct = -Infinity;
    
    for (let i = 0; i < nums.length; i++) {
        let product = 1;
        for (let j = i; j < nums.length; j++) {
            product *= nums[j];  // Calculate the product of the current subarray
            maxProduct = Math.max(maxProduct, product);  // Update maxProduct if a larger product is found
        }
    }
    return maxProduct;
}
```

#### Complexity:

- **Time Complexity:** O(n^2), where n is the number of elements in the array.
- **Space Complexity:** O(1), as we only use a few extra variables.

### Approach 2: Dynamic Programming

Instead of calculating every subarray product from scratch, we can consider the properties of multiplication: 

1. A positive product can be made larger by multiplying by a positive number (unless overflowed by a negative).
2. A negative product can turn positive if multiplied by another negative.
3. Thus, store both the maximum and minimum product up to each index.

#### Code:

```javascript
function maxProduct(nums) {
    if (nums.length === 0) return 0;
    
    let maxProduct = nums[0];
    let currentMax = nums[0];
    let currentMin = nums[0];
    
    for (let i = 1; i < nums.length; i++) {
        let tempMax = currentMax;
        
        // Update currentMax and currentMin for the next element
        currentMax = Math.max(nums[i], currentMax * nums[i], currentMin * nums[i]);
        currentMin = Math.min(nums[i], tempMax * nums[i], currentMin * nums[i]);
        
        maxProduct = Math.max(maxProduct, currentMax);
    }
    return maxProduct;
}
```

#### Complexity:

- **Time Complexity:** O(n), as we traverse the array once.
- **Space Complexity:** O(1), as we use a constant amount of space.

### Approach 3: Optimized Dynamic Programming (Two-pass with Cache)

In this approach, we utilize a forward pass and a backward pass to handle cases with negative numbers more effectively:

- First, compute the products from left to right, keeping track of the current maximum.
- Then compute the products from right to left to cater for cases where the maximum product is due to elements at the array's end.

#### Code:

```javascript
function maxProduct(nums) {
    if (nums.length === 0) return 0;

    let maxProduct = nums[0];
    let forwardProduct = 1;
    let backwardProduct = 1;
    
    for (let i = 0; i < nums.length; i++) {
        forwardProduct *= nums[i];  // forward pass
        backwardProduct *= nums[nums.length - 1 - i];  // backward pass
        
        maxProduct = Math.max(maxProduct, forwardProduct, backwardProduct);

        // Reset to 1 if there is a zero in the product path
        forwardProduct = forwardProduct === 0 ? 1 : forwardProduct;
        backwardProduct = backwardProduct === 0 ? 1 : backwardProduct;
    }
    
    return maxProduct;
}
```

#### Complexity:

- **Time Complexity:** O(n), as we do two passes through the array.
- **Space Complexity:** O(1), using only a fixed amount of extra space. 

These approaches begin with the simplest implementation and evolve into more efficient solutions, illustrating the thought process behind optimally solving the problem.

