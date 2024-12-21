# [Leetcode 1004: Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

## Approaches

1. [Sliding Window Approach](#sliding-window-approach)

### Sliding Window Approach

The problem essentially boils down to finding the longest subarray that can be obtained by flipping at most `k` zeros in the given binary array. A sliding window approach is appropriate here as it allows for dynamically adjusting the window size to contain as many ones as possible while allowing up to `k` zeros.

#### Intuition

1. **Window Expansion**: Use two pointers to represent the window. Start with both pointers at the beginning of the array.
2. **Counting Zeros**: As you expand the right pointer (to include elements in the window), count the number of zeros within this window.
3. **Window Contraction**: If the number of zeros within the window exceeds `k`, advance the left pointer to shrink the window from the left until the number of zeros is at most `k`.
4. **Max Length Calculation**: During each expansion, check if the current window size (from left to right pointers) is the largest so far and update the maximum length accordingly.
5. **Result**: The maximum length tracked is the answer to the problem.

This technique ensures we are checking each possible subarray with exactly one scan through the original array, adjusting the window dynamically, and thus achieving an efficient solution.

#### Code

```typescript
function longestOnes(nums: number[], k: number): number {
    let left = 0; // left pointer of the window
    let zerosCount = 0; // current count of zeros in the window
    let maxLength = 0; // maximum length of the window satisfying the condition

    for (let right = 0; right < nums.length; right++) {
        // expand the window by including nums[right]
        if (nums[right] === 0) {
            zerosCount++; // count the zero
        }

        // if zeroCount exceeds k, move left pointer to reduce zeros count
        while (zerosCount > k) {
            if (nums[left] === 0) {
                zerosCount--; // leaving a zero, decrease the count
            }
            left++; // move left pointer to shrink the window
        }

        // update the maximum length of the subarray
        maxLength = Math.max(maxLength, right - left + 1);
    }

    return maxLength;
}
```

#### Time and Space Complexity

- **Time Complexity**: O(N), where N is the number of elements in the array. Each element is processed at most twice (once by the right pointer and possibly once by the left pointer).
- **Space Complexity**: O(1), as we are using a constant amount of extra space regardless of input size.

This approach leverages the sliding window technique to efficiently solve the problem by allowing us to handle the problem constraints directly and flexibly manage the conditions needed to find the optimal solution.

