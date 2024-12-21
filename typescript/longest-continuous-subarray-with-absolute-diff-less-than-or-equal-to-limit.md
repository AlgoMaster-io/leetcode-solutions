## [Leetcode 1438: Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

### Approaches
1. [Brute Force](#brute-force)
2. [Sliding Window with Min-Max Deque](#sliding-window-with-min-max-deque)

### Brute Force

#### Intuition:
The brute force approach involves checking every possible subarray in the given array and determining if it meets the condition of the absolute difference between the smallest and largest element being less than or equal to the limit. Though simple, this approach can be very slow for large inputs.

#### Code:
```typescript
function longestSubarray(nums: number[], limit: number): number {
    let n = nums.length;
    let maxLength = 0;
    
    // Iterate over all possible starting points of the subarray
    for (let i = 0; i < n; i++) {
        let min = nums[i];
        let max = nums[i];
        
        // Explore all subarrays starting with i
        for (let j = i; j < n; j++) {
            min = Math.min(min, nums[j]);
            max = Math.max(max, nums[j]);
            
            // Check if the current subarray satisfies the condition
            if (max - min <= limit) {
                maxLength = Math.max(maxLength, j - i + 1);
            } else {
                break;  // If not, no need to expand this subarray further
            }
        }
    }
    
    return maxLength;
}
```

#### Time Complexity:
- **O(n^2)**: For each element, we iterate over the rest of the elements to check conditions.
  
#### Space Complexity:
- **O(1)**: We only use a fixed amount of extra space.

### Sliding Window with Min-Max Deque

#### Intuition:
Using sliding window technique combined with two deques (or a balanced tree structure), allows us to efficiently maintain the minimum and maximum of the current window. The deques help in maintaining the order of elements to quickly access the current window's min and max, thus allowing us to expand and contract the window optimally.

#### Code:
```typescript
function longestSubarray(nums: number[], limit: number): number {
    const minDeque: number[] = [];
    const maxDeque: number[] = [];
    let left = 0;
    let maxLength = 0;

    for (let right = 0; right < nums.length; right++) {
        // Maintain the minDeque so that it always keeps the smallest element at the front
        while (minDeque.length > 0 && nums[minDeque[minDeque.length - 1]] > nums[right]) {
            minDeque.pop();
        }
        minDeque.push(right);

        // Maintain the maxDeque so that it always keeps the largest element at the front
        while (maxDeque.length > 0 && nums[maxDeque[maxDeque.length - 1]] < nums[right]) {
            maxDeque.pop();
        }
        maxDeque.push(right);

        // Check if the current window is valid, otherwise slide the window from left
        while (nums[maxDeque[0]] - nums[minDeque[0]] > limit) {
            left++;
            // Clean out old indices from both deques
            if (minDeque[0] < left) {
                minDeque.shift();
            }
            if (maxDeque[0] < left) {
                maxDeque.shift();
            }
        }

        // Calculate the maximum length of the valid window
        maxLength = Math.max(maxLength, right - left + 1);
    }

    return maxLength;
}
```

#### Time Complexity:
- **O(n)**: Each element is added and removed from the deques at most once.

#### Space Complexity:
- **O(n)**: In the worst case, deques can store all elements.


