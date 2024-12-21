### [LeetCode 1004: Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

---

## Solutions:

- [Approach 1: Sliding Window with Two Pointers (Basic)](#approach-1)
- [Approach 2: Optimized Sliding Window (Most Optimal)](#approach-2)

---

### Approach 1: Sliding Window with Two Pointers (Basic)

**Intuition:**

The problem requires us to find the maximum number of consecutive 1's in an array if we can flip at most `k` 0's. The sliding window technique with two pointers allows us to form a window capturing a valid sequence of 1's (with at most `k` flips of 0's) while maintaining the necessary adjustments on window boundaries.

#### Steps:

1. Use two pointers, `left` and `right`, representing the current window of analysis.
2. Loop through the array extending the window by moving the `right` pointer.
3. While extending the window, count the number of 0's.
4. If the count of 0's exceeds `k`, shrink the window from the left until the window becomes valid (i.e., the count of 0's is `k` or less).
5. Record the maximum window size during these operations.

```csharp
public class Solution {
    public int LongestOnes(int[] nums, int k) {
        int left = 0, right = 0;
        int maxLen = 0;
        int zeroCount = 0;

        while (right < nums.Length) {
            // Expand the window by adding the right element
            if (nums[right] == 0) {
                zeroCount++;
            }
            
            // Shrink the window from the left if there are more than k zeroes
            while (zeroCount > k) {
                if (nums[left] == 0) {
                    zeroCount--;
                }
                left++;
            }
            
            // Update the maximum window size encountered
            maxLen = Math.Max(maxLen, right - left + 1);
            right++;
        }

        return maxLen;
    }
}
```

**Time Complexity:** O(n) where n is the length of the array. Each element is processed at most twice – once by `right` and once by `left`.

**Space Complexity:** O(1) since we are only using a few additional variables.

---

### Approach 2: Optimized Sliding Window (Most Optimal)

**Intuition:**

The solution can be optimized by avoiding unnecessary counts. We do not need an explicit count of 0's if we manage the window by adjusting based on the constraints (`right - left + 1 - zeroCount`) directly equating the feasible flips (k).

#### Steps:

- Similar to the above sliding window with two pointers.
- Directly compute the valid length of the window and adjust the `left` pointer based on permissible flips `k`.

```csharp
public class Solution {
    public int LongestOnes(int[] nums, int k) {
        int left = 0, zeroCount = 0;
        for (int right = 0; right < nums.Length; right++) {
            if (nums[right] == 0) {
                zeroCount++;
            }
            
            // Infrastructure to maintain the window valid
            if (zeroCount > k) {
                if (nums[left] == 0) {
                    zeroCount--;
                }
                left++;
            }
        }
        // Since the outermost loop final value `right` is effectively n, max window size is calculated 
        return nums.Length - left;
    }
}
```

**Time Complexity:** O(n) where n is the length of the array. Each element is processed at most twice. 

**Space Complexity:** O(1) since we're using a fixed amount of extra memory regardless of input size.

---

