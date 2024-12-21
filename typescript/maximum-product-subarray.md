# [Leetcode 152: Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming with Two Variables](#dynamic-programming-with-two-variables)

---

### Brute Force Approach

**Intuition:**  
The brute force approach involves considering every subarray and calculating its product. This approach will generate all possible subarrays and compute their products, tracking the maximum product found. While this approach is easy to understand, it is inefficient due to its high time complexity.

**Steps:**
1. Iterate through the array using two nested loops to consider every subarray.
2. For each subarray, calculate the product.
3. Keep track of the maximum product encountered so far.

**Code:**
```typescript
function maxProduct(nums: number[]): number {
    let maxProduct = -Infinity;
    const length = nums.length;

    for (let i = 0; i < length; ++i) {
        let product = 1;
        for (let j = i; j < length; ++j) {
            product *= nums[j];
            // Update the maximum product found so far
            maxProduct = Math.max(maxProduct, product);
        }
    }

    return maxProduct;
}
```

**Time Complexity:** O(n^2) – We use two nested loops to generate all subarrays.  
**Space Complexity:** O(1) – No extra space needed, just the variable to store the maximum product.

---

### Dynamic Programming with Two Variables

**Intuition:**  
The maximum product subarray problem is challenging because a subarray containing a zero can split the array into different products. Negative numbers complicate things since the product of two negatives is positive, which means the local minimum could become a maximum after a negative number.

To efficiently track the product, maintain two variables, `maxProd` and `minProd`, at each position:
- `maxProd` tracks the maximum product ending at the current position.
- `minProd` tracks the minimum product ending at the current position.

Here’s why we need both:
- A large negative product can become the largest positive product if another negative number is encountered.

**Steps:**
1. Initialize `maxProd`, `minProd`, and `globalMax` at the start.
2. Iterate through the array while updating `maxProd` and `minProd` using the current number.
3. The current `maxProd` is the maximum of:
   - The current number itself (single element subarray).
   - The product of the current number and the previous `maxProd`.
   - The product of the current number and the previous `minProd` (handles negative numbers).
4. Similarly update `minProd`.
5. Update the global maximum product as the loop progresses.

**Code:**
```typescript
function maxProduct(nums: number[]): number {
    let maxProd = nums[0];
    let minProd = nums[0];
    let globalMax = nums[0];

    for (let i = 1; i < nums.length; i++) {
        const currentNumber = nums[i];
        // Current max and min could be replaced by the results of multiplication with current number
        let tempMax = Math.max(currentNumber, maxProd * currentNumber, minProd * currentNumber);
        minProd = Math.min(currentNumber, maxProd * currentNumber, minProd * currentNumber);

        // Update maxProd with computed tempMax
        maxProd = tempMax;

        // Update global max
        globalMax = Math.max(globalMax, maxProd);
    }

    return globalMax;
}
```

**Time Complexity:** O(n) – We traverse the array once.  
**Space Complexity:** O(1) – Only a constant amount of additional space is used.

This method effectively finds the maximum product subarray with optimized time and space efficiency, addressing cases involving zero and negative numbers gracefully.

