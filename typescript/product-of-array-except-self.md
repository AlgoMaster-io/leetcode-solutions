# [Leetcode 238: Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Division](#approach-2-using-division)
- [Approach 3: Left and Right Products (Optimized)](#approach-3-left-and-right-products-optimized)

### Approach 1: Brute Force

#### Intuition
The brute-force approach involves computing the product for each index by multiplying all the elements of the array except for the element at that index. This can be done using two loops, where for each element, we iterate over the array to calculate the product of all other elements.

#### Code
```typescript
function productExceptSelf(nums: number[]): number[] {
    const length = nums.length;
    const result: number[] = new Array(length).fill(1);

    // Loop over each element to calculate the product excluding itself
    for (let i = 0; i < length; i++) {
        let product = 1;
        // Multiply every element except for nums[i]
        for (let j = 0; j < length; j++) {
            if (i !== j) {
                product *= nums[j];
            }
        }
        result[i] = product;
    }
    return result;
}
```

#### Complexity Analysis
- **Time Complexity:** O(n^2), where n is the length of the input array. For each element, we go through the entire list to multiply the other elements.
- **Space Complexity:** O(1), apart from the output array, no additional space is used.

### Approach 2: Using Division

#### Intuition
An optimization for the brute-force method is using the division operation. Calculate the product of all numbers and then divide the total product by each element to get the product of the rest. This method is easy but not applicable when there are zeros in the array (since dividing by zero is undefined).

#### Code
```typescript
function productExceptSelf(nums: number[]): number[] {
    const length = nums.length;
    let zeroCount = 0;
    let totalProduct = 1;

    // Calculate the total product of all non-zero elements
    for (let num of nums) {
        if (num !== 0) {
            totalProduct *= num;
        } else {
            zeroCount++;
        }
    }
    
    const result: number[] = new Array(length).fill(0);
    
    // If more than one zero, all products will be zero
    if (zeroCount > 1) return result;
    
    // Otherwise, calculate result based on zero count
    for (let i = 0; i < length; i++) {
        if (nums[i] !== 0) {
            result[i] = zeroCount === 0 ? totalProduct / nums[i] : 0;
        } else {
            result[i] = totalProduct;
        }
    }
    
    return result;
}
```

#### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the input array. We only traverse the array twice.
- **Space Complexity:** O(1), additional space used is minimal.

### Approach 3: Left and Right Products (Optimized)

#### Intuition
To avoid division and efficiently calculate the products, we can use left and right cumulative products. We first calculate all left products for each index, then all right products, and finally multiply them to get the desired product array. This method only makes two passes over the array and efficiently uses constant space.

#### Code
```typescript
function productExceptSelf(nums: number[]): number[] {
    const length = nums.length;
    const result: number[] = new Array(length).fill(1);

    // Calculate the left products
    let leftProduct = 1;
    for (let i = 0; i < length; i++) {
        result[i] = leftProduct;
        leftProduct *= nums[i];
    }

    // Calculate the right products and combine with left
    let rightProduct = 1;
    for (let i = length - 1; i >= 0; i--) {
        result[i] *= rightProduct;
        rightProduct *= nums[i];
    }

    return result;
}
```

#### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the input array. We make two passes over the array.
- **Space Complexity:** O(1), apart from the output array, no additional space is used as we're using the result array itself to store interim calculations.

This last approach is the most efficient and works in all cases without division.

