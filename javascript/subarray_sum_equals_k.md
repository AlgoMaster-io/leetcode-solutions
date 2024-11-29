# [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approach 1: Brute Force (Basic)

### Solution
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
function subarraySum(nums, k) {
    let count = 0;

    // Check all subarrays
    for (let i = 0; i < nums.length; i++) {
        let sum = 0;

        for (let j = i; j < nums.length; j++) {
            sum += nums[j];

            // If the sum equals k, increment the count
            if (sum === k) {
                count++;
            }
        }
    }

    return count;
}
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
function subarraySum(nums, k) {
    const prefixSumMap = new Map();
    prefixSumMap.set(0, 1); // Initialize with a prefix sum of 0

    let count = 0;
    let sum = 0;

    for (const num of nums) {
        sum += num; // Update the prefix sum

        // Check if there's a prefix sum that makes the current sum equal to k
        if (prefixSumMap.has(sum - k)) {
            count += prefixSumMap.get(sum - k);
        }

        // Update the frequency of the current prefix sum
        prefixSumMap.set(sum, (prefixSumMap.get(sum) || 0) + 1);
    }

    return count;
}
```

