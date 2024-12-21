# [Leetcode 209: Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Navigation
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointers / Sliding Window](#approach-2-two-pointers--sliding-window)

## Approach 1: Brute Force

### Intuition
The brute force approach involves generating all subarrays and checking if each subarray has a sum that is greater than or equal to the given `target`. We keep track of the minimum length of such subarrays.

### Solution
The brute force approach works by iterating through each element, and for each starting element, iterating through each possible ending element to form subarrays, checking their sums, and updating the minimum length.

```javascript
function minSubArrayLen(target, nums) {
    // Start with an impossibly high minimum length
    let minLength = Infinity;
    let n = nums.length;

    // Try every start index
    for (let start = 0; start < n; start++) {
        let sum = 0;
        // Try every end index for a given start
        for (let end = start; end < n; end++) {
            sum += nums[end];
            // Check if the current subarray meets the target
            if (sum >= target) {
                // Update minimum length found
                minLength = Math.min(minLength, end - start + 1);
                break; // No point in checking further, this end is optimal for this start
            }
        }
    }
    // If minLength hasn't changed, we didn't find any valid subarray
    return minLength === Infinity ? 0 : minLength;
}
```

### Time and Space Complexities
- **Time Complexity:** O(n^2), where `n` is the length of the array. This is due to the nested loops needed to evaluate each subarray.
- **Space Complexity:** O(1), as we are using a constant amount of extra space.

## Approach 2: Two Pointers / Sliding Window

### Intuition
The sliding window technique can be used to optimize this problem. We use two pointers to keep track of the current subarray and expand or shrink the window to optimize the subarray size that meets the sum requirement.

### Solution
- Initialize two pointers, `start` and `end`.
- Expand the `end` pointer to include new elements and compute the running sum.
- If the running sum is greater than or equal to the target, update the minLength and shrink the window from the `start` by incrementing it.

```javascript
function minSubArrayLen(target, nums) {
    let minLength = Infinity;
    let start = 0;
    let sum = 0;
    
    for (let end = 0; end < nums.length; end++) {
        // Add current element to the sum
        sum += nums[end];
        
        // Try to minimize the window size
        while (sum >= target) {
            // Update the minimum length
            minLength = Math.min(minLength, end - start + 1);
            // Shrink the window from the start
            sum -= nums[start];
            start++;
        }
    }
    
    // Return 0 if no such subarray exists
    return minLength === Infinity ? 0 : minLength;
}
```

### Time and Space Complexities
- **Time Complexity:** O(n), where `n` is the length of the array. Each element is visited at most twice, once by `end` to add to the sum and once by `start` to subtract from the sum.
- **Space Complexity:** O(1), as we are using a constant amount of extra space.

## Conclusion
The sliding window approach efficiently reduces the time complexity to linear time O(n) by maintaining a running sum and dynamically adjusting the window size, making it the preferred solution for this type of problem.

