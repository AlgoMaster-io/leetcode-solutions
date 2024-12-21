# [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach using Dynamic Programming](#optimized-approach-using-dynamic-programming)
3. [Most Optimal Solution using Space Optimization](#most-optimal-solution-using-space-optimization)

### Brute Force Approach

**Intuition**

The simplest approach is to consider every possible subarray of the given array, calculate the product of each, and keep track of the maximum product found. This ensures that we don't miss any potential maximum product subarray, but it is highly inefficient for large arrays due to its high time complexity.

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int maxProduct(vector<int>& nums) {
    int n = nums.size();
    int maxProduct = INT_MIN;

    // Consider each subarray starting with each element
    for(int i = 0; i < n; i++) {
        int product = 1;

        // Calculate product for each subarray beginning with nums[i]
        for(int j = i; j < n; j++) {
            product *= nums[j];
            // Update the maximum product found so far
            maxProduct = max(maxProduct, product);
        }
    }

    return maxProduct;
}
```

**Time Complexity**: O(n^2)  
We have two nested loops over n elements to try all subarrays.

**Space Complexity**: O(1)  
We are using a few extra variables.

---

### Optimized Approach using Dynamic Programming

**Intuition**

The idea is to use Dynamic Programming to keep track of the maximum and minimum product up to each point. This is because a negative number could become positive if multiplied by another negative number, and a previously minimum product could become the maximum if a new negative number is encountered.

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int maxProduct(vector<int>& nums) {
    int n = nums.size();

    // Initialize max and min product for the first element
    int maxProd = nums[0];
    int minProd = nums[0];
    // Result stores the maximum product found so far
    int result = nums[0];

    for (int i = 1; i < n; i++) {
        // Store current max in a temporary variable
        int tempMax = maxProd;
        // Update max and min product considering the current element
        maxProd = max({nums[i], maxProd * nums[i], minProd * nums[i]});
        minProd = min({nums[i], tempMax * nums[i], minProd * nums[i]});
        
        // Update the result with the maximum product found
        result = max(result, maxProd);
    }

    return result;
}
```

**Time Complexity**: O(n)  
We iterate through the array once.

**Space Complexity**: O(1)  
Only a constant amount of additional space is used.

---

### Most Optimal Solution using Space Optimization

**Intuition**

We can further simplify the space by updating the variables in place. The idea remains the same as that of the optimized approach using Dynamic Programming, but we directly use variables instead of an array to track the max and min products.

```cpp
#include <vector>
#include <algorithm>
using namespace std;

int maxProduct(vector<int>& nums) {
    int n = nums.size();

    // Initialize max and min product for the first element
    int maxProd = nums[0], minProd = nums[0];
    int result = nums[0];

    for (int i = 1; i < n; i++) {
        // Calculate current max and min product including nums[i]
        if (nums[i] < 0) {
            // Swap maxProd and minProd when nums[i] is negative
            swap(maxProd, minProd);
        }
        
        maxProd = max(nums[i], maxProd * nums[i]);
        minProd = min(nums[i], minProd * nums[i]);
        
        // Update result with the maximum product found
        result = max(result, maxProd);
    }

    return result;
}
```

**Time Complexity**: O(n)  
We iterate through the array once.

**Space Complexity**: O(1)  
Only a constant number of variables are used, optimizing space usage.

