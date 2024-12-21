# [Leetcode 2348: Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)

## Table of Contents

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointers (Optimal)](#approach-2-two-pointers-optimal)

---

## Approach 1: Brute Force

### Intuition:
The brute force approach involves checking every possible subarray and counting the ones that are completely filled with zeros. We iterate over each element, starting a subarray and expanding it until the point where the subarray is no longer zero-filled. This method will work but is not efficient and is mainly used for understanding the problem.

### Algorithm:

1. Initialize a counter `count` to keep track of zero-filled subarrays.
2. Use nested loops. The outer loop starts a subarray at every position.
3. The inner loop extends the subarray and checks if all elements are zero.
4. For each zero-filled subarray found, increment the counter `count`.
5. Return `count`.

### Code:

```javascript
function zeroFilledSubarray(nums) {
    let count = 0;
    
    // Loop over every starting point
    for (let start = 0; start < nums.length; start++) {
        // Initialize the current subarray sum
        let isZeroArray = true;
        
        // Loop to check each subarray starting at `start`
        for (let end = start; end < nums.length; end++) {
            if (nums[end] !== 0) {
                isZeroArray = false;
                break;
            }
            // If is a zero-filled subarray, count it
            if (isZeroArray) {
                count++;
            }
        }
    }

    return count;
}
```

### Time Complexity: 
- O(n^2), where n is the number of elements in the array, as we are checking each subarray.

### Space Complexity:
- O(1), as no extra space is used apart from variables.

---

## Approach 2: Two Pointers (Optimal)

### Intuition:
A more optimal approach takes advantage of the fact that if we find a subarray filled with zeros of length `k`, the number of zero-filled subarrays there is 1 + 2 + ... + k. This is equivalent to the sum of first `k` numbers, which is `k * (k + 1) / 2`. By counting consecutive zeros, we can jump directly to the solution without needing nested loops.

### Algorithm:

1. Initialize `count` and `currentZeros` to 0 to track zero-filled subarrays and current streak of zeros.
2. Iterate over the array.
3. If the current number is 0, increment `currentZeros`.
4. Otherwise, add the number of subarrays formed by the streak of zeros (`currentZeros * (currentZeros + 1) / 2`) to `count`, and reset `currentZeros` to 0.
5. After the loop, add any remaining `currentZeros` subarrays to `count`.
6. Return `count`.

### Code:

```javascript
function zeroFilledSubarray(nums) {
    let count = 0;
    let currentZeros = 0;
    
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] === 0) {
            // Increase current zero streak
            currentZeros++;
        } else {
            // Calculate the number of zero-filled subarrays for the streak
            count += (currentZeros * (currentZeros + 1)) / 2;
            // Reset current zero streak
            currentZeros = 0;
        }
    }
    
    // Add any remaining zero-filled subarrays
    count += (currentZeros * (currentZeros + 1)) / 2;
    
    return count;
}
```

### Time Complexity: 
- O(n), where n is the number of elements in the array, as we are iterating through the array once.

### Space Complexity:
- O(1), as no additional space is required.

---

