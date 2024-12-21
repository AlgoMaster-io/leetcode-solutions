# [Leetcode 2348: Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)

## Approaches
1. [Brute Force](#brute-force)
2. [Optimized: Two Pointers](#optimized-two-pointers)

### Brute Force

The brute force approach involves iterating over all possible subarrays of the array, and checking if each subarray is filled entirely with zeros. While this method works, it is not optimal as it involves a lot of repeated calculations.

**Intuition:**

- We generate all subarrays and check if they are zero-filled.
- For each zero-filled subarray, we increase a count.
- The brute force approach has a high time complexity due to nested loop iterations.

**Code:**

```typescript
function zeroFilledSubarrayBruteForce(nums: number[]): number {
    let count = 0;

    // Iterate through each starting point of subarrays
    for (let start = 0; start < nums.length; start++) {
        // Start a new subarray with zero length
        let isZeroFilled = true;
        
        // Extend the subarray to the right
        for (let end = start; end < nums.length; end++) {
            // If any non-zero is found, the subarray is not zero-filled
            if (nums[end] !== 0) {
                isZeroFilled = false;
                break;
            } else {
                // If zero, increment the count
                if(isZeroFilled) count++;
            }
        }
    }
    return count;
}

// Time Complexity: O(n^2)
// Space Complexity: O(1)
```

### Optimized: Two Pointers

Realizing the redundancy in the brute force approach, we can improve efficiency using a single pass with two pointers. The key insight is to count the length of contiguous segments of zeros and utilize the arithmetic sum to count zero-filled subarrays.

**Intuition:**

- Using a single pass, identify segments of contiguous zeros.
- For each contiguous zero segment of length `k`, there are `k * (k + 1) / 2` subarrays.
- This approach avoids nested iteration by directly counting zero segments.

**Code:**

```typescript
function zeroFilledSubarray(nums: number[]): number {
    let count = 0, zeroLength = 0;
    
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] === 0) {
            // Increase zero-segment length
            zeroLength++;
            // Add the number of new zero-subarrays with this zero
            count += zeroLength;
        } else {
            // Reset zero-segment length if non-zero is encountered
            zeroLength = 0;
        }
    }
    return count;
}

// Time Complexity: O(n)
// Space Complexity: O(1)
```

In this approach, every zero contributes to forming more zero-filled subarrays, and this is efficiently computed using a single traversal. The arithmetic sum accounts for all possible subarrays formed with each new zero in a contiguous segment.

