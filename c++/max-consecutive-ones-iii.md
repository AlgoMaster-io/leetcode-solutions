# [LeetCode 1004: Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

## Approaches:
- [Approach 1: Sliding Window](#approach-1-sliding-window)

## Approach 1: Sliding Window

The problem is a classical 'sliding window' problem where we are asked to find the longest contiguous subarray consisting of only `1`s after replacing at most `k` zeros with ones.

### Intuition:
1. **Goal**: Maximize the length of the subarray with ones after flipping `k` zeros.
2. **Sliding Window Technique**: This problem can be efficiently solved using a two-pointer approach (sliding window technique). We maintain a window with a left and a right boundary.
3. **Window Validity**: As we expand the right end of the window to include more elements, if the number of zeros in the window exceeds `k`, we move the left end of the window to the right to decrease the number of zeros.
4. **Optimal Window**: At any point, the window represents the longest valid subarray (after flipping at most `k` zeros) encountered so far.

### Steps:
- Initialize two pointers `left` and `right` at the start of the array.
- Initialize a counter `zeroCount` to keep track of zeros in the current window.
- As the right pointer extends the window by moving to the right:
  - If the element at `right` is a zero, increment `zeroCount`.
  - If `zeroCount` exceeds `k`, move the `left` pointer rightwards until `zeroCount` is less than or equal to `k`.
- Update the maximum length of `1`s found so far.

### C++ Code Implementation:
```cpp
class Solution {
public:
    int longestOnes(std::vector<int>& nums, int k) {
        int left = 0, right = 0;
        int zeroCount = 0;
        int maxLength = 0;

        // Traverse the array using the right pointer
        while (right < nums.size()) {
            // Increment zeroCount if nums[right] is zero
            if (nums[right] == 0) {
                zeroCount++;
            }

            // If zeroCount exceeds k, shrink the window from the left
            while (zeroCount > k) {
                if (nums[left] == 0) {
                    zeroCount--;
                }
                left++;
            }

            // Calculate the maximum length of valid window encountered so far
            maxLength = std::max(maxLength, right - left + 1);

            // Expand the window by moving to the right
            right++;
        }

        return maxLength;
    }
};
```

### Complexity Analysis:
- **Time Complexity**: O(n), where n is the length of the input array. Each element is processed at most twice (once by the `right` pointer and once by the `left` pointer).
- **Space Complexity**: O(1), as no additional space proportional to the input size is used.

This sliding window approach efficiently finds the length of the maximum subarray containing only `1`s that can be obtained by flipping at most `k` zeros.

With these steps and implementation, you should be able to solve this problem optimally with a clear understanding of each step involved.

