# [Leetcode 238: Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Approaches
1. [Simple Approach with Division (Inefficient due to constraints)](#approach-1)
2. [Left and Right Product Arrays](#approach-2)
3. [Optimized Left and Right Product (In-Place Calculation)](#approach-3)

---

## Approach 1: Simple Approach with Division (Inefficient due to constraints)

### Intuition
The straightforward way to solve this problem is to calculate the total product of all elements and then divide by each individual element to get the product of the array except for that element. However, this approach fails when `nums` contains zeros, and it's against the problem constraints of not using division.

### Approach
1. Calculate the total product of all numbers.
2. Iterate over `nums` and divide the total product by each element to get the result.
3. Handle zero separately (if division were allowed).

### Code
This approach is given for understanding but is not implemented here due to constraint violations.

### Time Complexity
- O(n)

### Space Complexity
- O(1) If not counting the output array.

---

## Approach 2: Left and Right Product Arrays

### Intuition
We can calculate the product of all elements except the current one without using division by using two auxiliary arrays:
- `leftProduct`: which at each index `i` contains the product of all the elements to the left of `i`.
- `rightProduct`: which at each index `i` contains the product of all the elements to the right of `i`.

The final result at index `i` will be the product of `leftProduct[i]` and `rightProduct[i]`.

### Approach
1. Initialize two arrays `leftProduct` and `rightProduct`, each of size `n`.
2. Fill `leftProduct` such that `leftProduct[i]` holds the product of all elements to the left of `i`.
3. Fill `rightProduct` such that `rightProduct[i]` holds the product of all elements to the right of `i`.
4. Calculate the result for each index as the product of `leftProduct[i]` and `rightProduct[i]`.

### Code
```go
func productExceptSelf(nums []int) []int {
    n := len(nums)
    leftProduct := make([]int, n)
    rightProduct := make([]int, n)
    result := make([]int, n)
    
    // Fill leftProduct array
    leftProduct[0] = 1
    for i := 1; i < n; i++ {
        leftProduct[i] = leftProduct[i-1] * nums[i-1]
    }
    
    // Fill rightProduct array
    rightProduct[n-1] = 1
    for i := n-2; i >= 0; i-- {
        rightProduct[i] = rightProduct[i+1] * nums[i+1]
    }
    
    // Fill result array
    for i := 0; i < n; i++ {
        result[i] = leftProduct[i] * rightProduct[i]
    }
    
    return result
}
```

### Time Complexity
- O(n)

### Space Complexity
- O(n) for the left and right products.

---

## Approach 3: Optimized Left and Right Product (In-Place Calculation)

### Intuition
We can optimize the space complexity of the previous solution by calculating the right products in the output array directly. This allows us to maintain the O(1) space complexity beside the output array.

### Approach
1. Initialize the result array with size `n`.
2. Use a single variable for the left product.
3. Traverse the array from left to right to fill the result with left products.
4. Then traverse the array from right to left, updating the result with right products.

### Code
```go
func productExceptSelf(nums []int) []int {
    n := len(nums)
    result := make([]int, n)
    
    // Initialize result array with 1
    result[0] = 1
    for i := 1; i < n; i++ {
        result[i] = result[i-1] * nums[i-1]
    }
    
    // Use a variable to keep track of the right product
    right := 1
    for i := n-1; i >= 0; i-- {
        result[i] *= right
        right *= nums[i]
    }
    
    return result
}
```

### Time Complexity
- O(n)

### Space Complexity
- O(1) Additional space aside from the output array.

