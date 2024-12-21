# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Approaches:
- [Brute-force Approach](#brute-force-approach)
- [Sliding Window Approach](#sliding-window-approach)

### Brute-force Approach

**Intuition:**
The brute-force approach involves checking every possible subarray in the given array and determining its sum. If a subarray meets or exceeds the target sum, we compare its length with the shortest length found so far. This approach ensures that we do not miss any potential subarray which meets the requirement.

**Code:**

```typescript
function minSubArrayLen(target: number, nums: number[]): number {
    const n = nums.length;
    let minLength = Infinity; // Initialize with a very large number

    // Consider every possible subarray
    for (let start = 0; start < n; start++) {
        let sum = 0;
        for (let end = start; end < n; end++) {
            sum += nums[end]; // Add the current number to the sum
            
            // Check if the current sum is equal or larger than the target
            if (sum >= target) {
                minLength = Math.min(minLength, end - start + 1);
                break; // Break early since no need to go further for this start
            }
        }
    }

    return minLength === Infinity ? 0 : minLength;
}
```

**Time Complexity:** O(n^2) because we are using a nested loop where each subarray start leads to a full scan ahead.

**Space Complexity:** O(1) as we are using only a fixed amount of extra space.

### Sliding Window Approach

**Intuition:**
The sliding window technique helps optimize the previous approach by keeping a running sum of the elements in a window and adjusting the window's size dynamically. We expand the window by adding elements from the right and contract by removing elements from the left once the current window's sum meets or exceeds the target. This allows us to efficiently find the minimum length subarray.

**Code:**

```typescript
function minSubArrayLen(target: number, nums: number[]): number {
    const n = nums.length;
    let minLength = Infinity; // Initialize the shortest length with a large number
    let left = 0; // Start index of the sliding window
    let sum = 0; // Current sum of the window

    for (let right = 0; right < n; right++) {
        sum += nums[right]; // Add the rightmost element to the current sum

        // Shrink the window from the left as much as possible
        while (sum >= target) {
            minLength = Math.min(minLength, right - left + 1);
            sum -= nums[left]; // Remove the leftmost element from the sum
            left++; // Move the left boundary of the window to the right
        }
    }

    return minLength === Infinity ? 0 : minLength;
}
```

**Time Complexity:** O(n) as each element is processed only a few times through the right and left pointers.

**Space Complexity:** O(1) as we use a fixed amount of extra space regardless of the input size.

