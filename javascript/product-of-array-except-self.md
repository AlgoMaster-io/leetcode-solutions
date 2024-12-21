[Leetcode 238: Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dividing Total Product by Element](#approach-2-dividing-total-product-by-element)
- [Approach 3: Using Prefix and Suffix Products](#approach-3-using-prefix-and-suffix-products)

### Approach 1: Brute Force

#### Intuition
The brute force approach involves calculating the product of array elements for each index by multiplying all other elements except the current one. This directly gives the desired result but is inefficient for large arrays due to its high time complexity.

#### Code
```javascript
var productExceptSelf = function(nums) {
    let length = nums.length;
    let result = new Array(length).fill(1);
    
    for (let i = 0; i < length; i++) {
        let product = 1;
        for (let j = 0; j < length; j++) {
            // If it is not the current index, multiply
            if (i !== j) {
                product *= nums[j];
            }
        }
        // Assign the computed product to result array
        result[i] = product;
    }
    
    return result;
};
```

#### Time Complexity
- **Time**: O(n^2), because for each element, we traverse the entire array.

#### Space Complexity
- **Space**: O(1), aside from the space used for the output array.

---

### Approach 2: Dividing Total Product by Element

#### Intuition
Calculate the total product of all elements and then divide this product by each element to get the desired result. This works unless the array contains zeros. Therefore, this solution might fail for a larger number of zeros as division by zero is undefined.

#### Code
```javascript
var productExceptSelf = function(nums) {
    let length = nums.length;
    let totalProduct = 1;
    let zeroCount = 0;
    
    // Compute the total product and count zeros
    for (let num of nums) {
        if (num !== 0) {
            totalProduct *= num;
        } else {
            zeroCount++;
        }
    }

    let result = new Array(length).fill(0);
    
    for (let i = 0; i < length; i++) {
        if (nums[i] === 0) {
            // If more than one zero, all results should be zero
            if (zeroCount > 1) {
                result[i] = 0;
            } else {
                result[i] = totalProduct; // Only one zero, the current position should have total product
            }
        } else {
            if (zeroCount > 0) {
                result[i] = 0; // If zero exists elsewhere, product is zero
            } else {
                result[i] = totalProduct / nums[i]; // Normal division
            }
        }
    }
    
    return result;
};
```

#### Time Complexity
- **Time**: O(n), a single pass to calculate product and another to fill the result array.

#### Space Complexity
- **Space**: O(n), to store the result.

---

### Approach 3: Using Prefix and Suffix Products

#### Intuition
The core idea is to utilize prefix and suffix arrays to calculate the product except for the current element without directly dividing by zero. We build the prefix product up to each point and the suffix product from each point, and multiply these to get the required product for each index.

#### Code
```javascript
var productExceptSelf = function(nums) {
    let length = nums.length;
    let result = new Array(length);
    
    let prefix = 1;
    for (let i = 0; i < length; i++) {
        result[i] = prefix;
        prefix *= nums[i];
    }

    let suffix = 1;
    for (let i = length - 1; i >= 0; i--) {
        result[i] *= suffix;
        suffix *= nums[i];
    }
    
    return result;
};
```

#### Time Complexity
- **Time**: O(n), single traversal for prefix and another reverse traversal for suffix.

#### Space Complexity
- **Space**: O(1), as we are utilizing the result array for storing prefix and suffix products and not using any additional space for these products outside of the result array.

---

