# [2461. Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

## Approach 1: Brute Force (Basic)

### Solution
```javascript
// Time Complexity: O(n * k)
// Space Complexity: O(k)

function maximumSubarraySum(nums, k) {
    let maxSum = 0;

    // Iterate over every subarray of size k
    for (let i = 0; i <= nums.length - k; i++) {
        const uniqueElements = new Set();
        let sum = 0;
        let isValid = true;

        for (let j = i; j < i + k; j++) {
            if (uniqueElements.has(nums[j])) {
                isValid = false; // Subarray contains duplicate
                break;
            }
            uniqueElements.add(nums[j]);
            sum += nums[j];
        }

        if (isValid) {
            maxSum = Math.max(maxSum, sum);
        }
    }

    return maxSum;
}
```

## Approach 2: Sliding Window with HashMap (Optimal)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(k)

function maximumSubarraySum(nums, k) {
    let maxSum = 0, currentSum = 0;
    const frequencyMap = new Map();
    let left = 0;

    // Sliding window
    for (let right = 0; right < nums.length; right++) {
        currentSum += nums[right];
        frequencyMap.set(nums[right], (frequencyMap.get(nums[right]) || 0) + 1);

        // If the window size exceeds k, shrink it
        if (right - left + 1 > k) {
            currentSum -= nums[left];
            frequencyMap.set(nums[left], frequencyMap.get(nums[left]) - 1);
            if (frequencyMap.get(nums[left]) === 0) {
                frequencyMap.delete(nums[left]);
            }
            left++;
        }

        // Check if the current window is valid (distinct elements)
        if (right - left + 1 === k && frequencyMap.size === k) {
            maxSum = Math.max(maxSum, currentSum);
        }
    }

    return maxSum;
}
```

