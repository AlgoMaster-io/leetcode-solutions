# [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Navigation
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Prefix and Suffix Products](#approach-2-prefix-and-suffix-products)
- [Approach 3: Optimized with Single Pass](#approach-3-optimized-with-single-pass)

## Approach 1: Brute Force

### Intuition
The brute force approach is to calculate the product of all elements in the array except the one at the current index. We can achieve this by iterating over each element and using an inner loop to calculate the product of all elements except the current one.

### Steps
1. Initialize an output array of length `n` with all elements set to 1.
2. For each element in the input array, iterate over the input array again to calculate the product of all elements except the current one.
3. Assign this product to the corresponding index in the output array.

### Code
```java
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] output = new int[n];

    for (int i = 0; i < n; i++) {
        output[i] = 1;  // Initialize each element as 1 for multiplication
        for (int j = 0; j < n; j++) {
            if (i != j) {
                output[i] *= nums[j];
            }
        }
    }

    return output;
}
```

### Time and Space Complexity
- **Time Complexity:** O(n^2) because for each element, we loop through the array again.
- **Space Complexity:** O(1) except for the output array, which does not count as extra space by convention.

## Approach 2: Prefix and Suffix Products

### Intuition
To improve efficiency, we can use two auxiliary arrays to store the product of all elements to the left and to the right of each index. This way, we can construct the result array by multiplying the prefix and suffix products.

### Steps
1. Create two arrays `prefix` and `suffix`, both of size `n`.
2. Fill the `prefix` array such that `prefix[i]` holds the product of all elements to the left of `i`.
3. Fill the `suffix` array such that `suffix[i]` holds the product of all elements to the right of `i`.
4. For the result array, multiply the corresponding values of `prefix` and `suffix`.

### Code
```java
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] output = new int[n];
    int[] prefix = new int[n];
    int[] suffix = new int[n];

    prefix[0] = 1;
    for (int i = 1; i < n; i++) {
        prefix[i] = prefix[i - 1] * nums[i - 1];
    }

    suffix[n - 1] = 1;
    for (int i = n - 2; i >= 0; i--) {
        suffix[i] = suffix[i + 1] * nums[i + 1];
    }

    for (int i = 0; i < n; i++) {
        output[i] = prefix[i] * suffix[i];
    }

    return output;
}
```

### Time and Space Complexity
- **Time Complexity:** O(n) because we are traversing the input array three times.
- **Space Complexity:** O(n) for the prefix and suffix arrays.

## Approach 3: Optimized with Single Pass

### Intuition
To optimize our space usage, we can eliminate the suffix array and calculate the right product on the fly while filling up the result array. We maintain a variable `right` initially set to 1, which is used to accumulate the product of elements to the right. We first fill the result with the left products, then iterate backward and update with the `right` product.

### Steps
1. Initialize the result array by computing the prefix (left) products as in approach 2.
2. Use a single variable `right` to iterate from the end of the array, updating the result by multiplying with `right`.
3. Update `right` in each iteration as you progress from right to left.

### Code
```java
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] output = new int[n];

    // Initialize the output array with left products
    output[0] = 1;
    for (int i = 1; i < n; i++) {
        output[i] = output[i - 1] * nums[i - 1];
    }

    // Calculate and multiply the right products
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        output[i] *= right;
        right *= nums[i];
    }

    return output;
}
```

### Time and Space Complexity
- **Time Complexity:** O(n) because it involves two passes over the array.
- **Space Complexity:** O(1) extra space apart from the output array.

