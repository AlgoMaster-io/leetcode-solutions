# [Leetcode 238: Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Prefix and Suffix Product Approach](#prefix-and-suffix-product-approach)
3. [Optimal Space Prefix and Suffix Product Approach](#optimal-space-prefix-and-suffix-product-approach)

---

## Brute Force Approach

### Intuition:
The brute force approach involves calculating the product for each element by multiplying all the elements of the array and dividing by the current element. However, this approach is naive in the sense that it requires calculating product individually for each element which ends up being inefficient for larger datasets and might lead to division by zero when there are zeros in the array.

### Implementation:

```python
def productExceptSelf(nums):
    length = len(nums)
    # Initialize the output array to store the results
    output = [1] * length
    for i in range(length):
        prod = 1  # The product of all other elements except nums[i]
        for j in range(length):
            if i != j:
                prod *= nums[j]
        output[i] = prod
    return output

# Test the function with an example
print(productExceptSelf([1, 2, 3, 4]))  # Output: [24, 12, 8, 6]
```

### Time Complexity:
- **Time:** \(O(n^2)\) - Because for each element, we iterate through the entire array to calculate the product.
- **Space:** \(O(n)\) - We are using an output list to store the results, but no additional space except for that.

---

## Prefix and Suffix Product Approach

### Intuition:
This approach reduces the complexity significantly by using an auxiliary array for prefix and suffix products. A prefix product is the product of all elements before a given index, and a suffix product is the product of all elements after a given index. We can then easily compute the desired product by multiplying the prefix and suffix products.

### Implementation:

```python
def productExceptSelf(nums):
    length = len(nums)
    # Initialize prefix and suffix arrays
    prefix = [1] * length
    suffix = [1] * length
    output = [1] * length

    # Calculate prefix products
    for i in range(1, length):
        prefix[i] = prefix[i - 1] * nums[i - 1]

    # Calculate suffix products
    for i in range(length - 2, -1, -1):
        suffix[i] = suffix[i + 1] * nums[i + 1]

    # Calculate the result by multiplying prefix and suffix
    for i in range(length):
        output[i] = prefix[i] * suffix[i]

    return output

# Test the function with an example
print(productExceptSelf([1, 2, 3, 4]))  # Output: [24, 12, 8, 6]
```

### Time Complexity:
- **Time:** \(O(n)\) - We traverse the list a few times to fill in the prefix and suffix arrays and to calculate the output.
- **Space:** \(O(n)\) - Due to the prefix and suffix arrays.

---

## Optimal Space Prefix and Suffix Product Approach

### Intuition:
To further optimize space, we can reuse the output array for storing the prefix products, while calculating the suffix product on the fly. This eliminates the need for extra space dedicated to suffix storage.

### Implementation:

```python
def productExceptSelf(nums):
    length = len(nums)
    # Initialize the output array for prefix products and final result
    output = [1] * length

    # Calculate prefix products directly into the output array
    for i in range(1, length):
        output[i] = output[i - 1] * nums[i - 1]

    # Initialize a suffix product variable
    suffix_product = 1
    for i in range(length - 1, -1, -1):
        # Multiply the suffix product with the current output product
        output[i] *= suffix_product
        # Update the suffix product for the next iteration
        suffix_product *= nums[i]

    return output

# Test the function with an example
print(productExceptSelf([1, 2, 3, 4]))  # Output: [24, 12, 8, 6]
```

### Time Complexity:
- **Time:** \(O(n)\) - Single pass through the list for calculating prefix and suffix products respectively.
- **Space:** \(O(1)\) - Except for the output array, no extra space is utilized for computations. 

This final approach gives us an optimized solution in terms of both time and space complexities while safely avoiding any zero division errors by never actually performing division.

