# [LeetCode 238: Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## Navigation
- [Approach 1: Using Division](#approach-1-using-division)
- [Approach 2: Two Passes with Extra Space](#approach-2-two-passes-with-extra-space)
- [Approach 3: Two Passes with Constant Space](#approach-3-two-passes-with-constant-space)

## Approach 1: Using Division

### Intuition
This approach leverages the mathematical property that the product of a series of numbers divided by any one of those numbers equals the product of the rest of the numbers. However, this approach cannot handle cases where zeros exist in the array and division would normally not be allowed on competitve programming platforms where integers are involved.

### Algorithm
1. Compute the total product of all elements.
2. For each element in the array, compute the product of array except itself by dividing the total product by the element.

### C++ Code
```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    // Calculate the total product of all elements
    int totalProduct = 1;
    for (int num : nums) {
        totalProduct *= num;
    }
    
    // Create a result array where each element is the total product divided by the element
    vector<int> result(nums.size());
    for (int i = 0; i < nums.size(); ++i) {
        result[i] = totalProduct / nums[i];
    }
    
    return result;
}
```
**Time Complexity:** O(n)  
**Space Complexity:** O(n) because the result array uses extra space.

**Note:** This approach does not handle cases with zeros due to division, hence it is often rejected on platforms like LeetCode.

## Approach 2: Two Passes with Extra Space

### Intuition
Instead of using division, we can calculate the product of all the numbers on the left and right of each number, excluding itself.

### Algorithm
1. Create two auxiliary arrays `L` and `R`, where `L[i]` contains the product of all the elements to the left of `i` and `R[i]` contains the product of all the elements to the right of `i`.
2. Calculate `L` and `R` in two separate passes.
3. Compute the result by multiplying `L[i]` and `R[i]` for each `i`.

### C++ Code
```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> L(n), R(n), result(n);
    
    // Building the L array
    L[0] = 1;
    for (int i = 1; i < n; ++i) {
        L[i] = L[i - 1] * nums[i - 1];
    }

    // Building the R array
    R[n - 1] = 1;
    for (int i = n - 2; i >= 0; --i) {
        R[i] = R[i + 1] * nums[i + 1];
    }

    // Computing the result by multiplying L and R
    for (int i = 0; i < n; ++i) {
        result[i] = L[i] * R[i];
    }
    
    return result;
}
```
**Time Complexity:** O(n)  
**Space Complexity:** O(n) for the extra `L` and `R` arrays.

## Approach 3: Two Passes with Constant Space

### Intuition
To optimize the space, we can reuse the `result` array for one of the passes instead of separate arrays for left and right products.

### Algorithm
1. Use the `result` array to store the product of all elements to the left.
2. Traverse the array in reverse to compute the product of elements to the right updating the result in place using a running product.

### C++ Code
```cpp
vector<int> productExceptSelf(vector<int>& nums) {
    int n = nums.size();
    vector<int> result(n);
    
    // Calculate L array on the fly in result
    result[0] = 1;
    for (int i = 1; i < n; ++i) {
        result[i] = result[i - 1] * nums[i - 1];
    }
    
    // R represents the running product of elements to the right
    int R = 1;
    for (int i = n - 1; i >= 0; --i) {
        result[i] *= R;
        R *= nums[i];
    }
    
    return result;
}
```
**Time Complexity:** O(n)  
**Space Complexity:** O(1) if we do not count the output array as extra space, which is the constraint of the problem.

