# [Leetcode 643: Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Approaches:

1. [Brute Force Approach](#brute-force-approach)
2. [Sliding Window Approach](#sliding-window-approach)

### Brute Force Approach

#### Intuition:
The brute force approach involves calculating the average of every possible contiguous subarray of length `k`, and keeping track of the maximum average found. This involves checking every possible subarray of size `k` for the given `nums` array.

#### Approach:
1. Traverse the array from the start to the point where the last subarray of size `k` would end. This is the length of the array minus `k`.
2. For each starting index `i`, calculate the sum of the subarray starting at `i` and of length `k`.
3. Calculate the average by dividing the sum by `k`.
4. Keep track of the maximum average found.

#### Time Complexity:
- **O(n*k)**: We iterate through the array up to `n-k` and for each position, we compute the sum of `k` elements.
 
#### Space Complexity:
- **O(1)**: We use a fixed amount of extra space not dependent on the input size.

#### JavaScript Code:

```javascript
function findMaxAverage(nums, k) {
    let maxAverage = -Infinity;
    
    // Traverse through each possible starting point of the subarray
    for (let i = 0; i <= nums.length - k; i++) {
        let currentSum = 0;
        
        // Calculate the sum of current subarray of length k
        for (let j = i; j < i + k; j++) {
            currentSum += nums[j];
        }
        
        // Calculate the average and update maxAverage
        let currentAverage = currentSum / k;
        if (currentAverage > maxAverage) {
            maxAverage = currentAverage;
        }
    }
    
    return maxAverage;
}
```

### Sliding Window Approach

#### Intuition:
The sliding window approach optimizes the brute force solution. Instead of recalculating the sum from scratch each time, we slide the window one element forward and update the sum by subtracting the element that is leaving the window and adding the new element. This reduces the time complexity significantly.

#### Approach:
1. Initialize the sum of the first `k` elements.
2. Traverse the array from the `k-th` element to the end, sliding the window one element at a time.
3. Update the sum by subtracting the element that just went out of the window and adding the new element that comes into the window.
4. Calculate the average and update the maximum average found.
5. Return the maximum average at the end.

#### Time Complexity:
- **O(n)**: We iterate through the array once, updating the sum in constant time for each step.
 
#### Space Complexity:
- **O(1)**: We use a fixed amount of extra space not dependent on the input size.

#### JavaScript Code:

```javascript
function findMaxAverage(nums, k) {
    // Calculate the sum of the first window of size k
    let currentSum = 0;
    for (let i = 0; i < k; i++) {
        currentSum += nums[i];
    }
    
    // This will also be the maximum sum initially
    let maxSum = currentSum;
    
    // Start from the next element after the first window
    for (let i = k; i < nums.length; i++) {
        // Slide the window by removing the element going out of the window
        // and adding the new element into the current window
        currentSum = currentSum - nums[i - k] + nums[i];
        
        // Update maxSum if the currentSum of this window is larger
        if (currentSum > maxSum) {
            maxSum = currentSum;
        }
    }
    
    // Return the maximum average which is maxSum divided by k
    return maxSum / k;
}
```

These two approaches provide a comprehensive solution for the "Maximum Average Subarray I" problem, starting from the basic brute force to the more efficient sliding window technique.

