# [Leetcode 1004: Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

## Approaches
- [Sliding Window Approach](#sliding-window-approach)

### Sliding Window Approach

The problem requires us to find the longest subarray with all 1's that can be obtained by flipping at most `k` 0's to 1's. A sliding window is a suitable method to solve this because it allows us to efficiently monitor how many 0's have been used in the current window.

**Intuition:**
1. We use a sliding window to explore potential maximum subarrays of 1's.
2. Initialize two pointers `left` and `right` at the start of the array. Moreover, we will maintain a counter `zeroCount` to count the number of zeros in the current window.
3. Expand the window by moving `right` pointer and logging any zeros we encounter.
4. Whenever `zeroCount` exceeds `k`, move the `left` pointer to reduce the number of zeros in the window. This adjustment will ensure `zeroCount` remains within the permissible limit.
5. At each step, compute the window size as `(right - left + 1)` and track the maximum size found.
6. The final result will be the largest possible window size which adheres to the constraints.

**Algorithm in Words:**
- Initialize `left` and `zeroCount`.
- Traverse through the array with `right`.
  - If `nums[right]` is 0, increment `zeroCount`.
  - If `zeroCount` exceeds `k`, increment `left` until `zeroCount` is valid.
  - Calculate the current window size and keep track of the maximum size.
- Return the maximum size tracked.

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var longestOnes = function(nums, k) {
    let left = 0;
    let zeroCount = 0;
    let maxConsecutiveOnes = 0;

    // Traverse the array using right pointer
    for (let right = 0; right < nums.length; right++) {
        // If we encounter a 0, increase zeroCount
        if (nums[right] === 0) {
            zeroCount += 1;
        }

        // If zeroCount exceeds k, increment left pointer until it's valid
        while (zeroCount > k) {
            if (nums[left] === 0) {
                zeroCount -= 1;
            }
            left += 1;
        }

        // Calculate the window size and update maxConsecutiveOnes
        maxConsecutiveOnes = Math.max(maxConsecutiveOnes, right - left + 1);
    }

    return maxConsecutiveOnes;
};
```

**Complexity Analysis:**
- **Time Complexity:** O(n), where n is the length of the input array. The `right` pointer traverses each element once, and the `left` pointer only advances from the starting point to the current `right` as needed.
- **Space Complexity:** O(1), as only a few extra variables are used regardless of the input size.

