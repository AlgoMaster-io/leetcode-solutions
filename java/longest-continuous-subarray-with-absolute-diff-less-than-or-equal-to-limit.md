# [Leetcode 1438: Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized Sliding Window with Two Deques](#approach-2-optimized-sliding-window-with-two-deques)

## Approach 1: Brute Force

### Intuition
The simplest way to solve this problem is by checking every possible subarray and verifying if it meets the condition, i.e., its maximum and minimum difference is less than or equal to the given limit. This involves calculating the max and min for all subarrays which can be computationally expensive but forms the foundation of understanding the problem.

### Algorithm
1. Iterate over each starting index of a subarray.
2. For each starting index, iterate over each possible ending index, maintaining the current maximum and minimum in the subarray.
3. If at any point the difference between the maximum and minimum exceeds the limit, move to the next starting index.
4. Keep track of the length of the longest valid subarray found.

### Implementation
```java
public class Solution {
    public int longestSubarray(int[] nums, int limit) {
        int n = nums.length;
        int maxLen = 0;
        
        // Check every possible subarray
        for (int start = 0; start < n; start++) {
            int max = nums[start];
            int min = nums[start];
            for (int end = start; end < n; end++) {
                // Update max and min in the current subarray
                max = Math.max(max, nums[end]);
                min = Math.min(min, nums[end]);
                
                // Check if the condition is met
                if (max - min <= limit) {
                    maxLen = Math.max(maxLen, end - start + 1);
                } else {
                    break;
                }
            }
        }
        return maxLen;
    }
}
```

### Time Complexity
- **O(N^2):** All possible subarrays are considered, each requiring potentially a scan from start to end.
  
### Space Complexity
- **O(1):** Only a few integer variables utilized for computation.

## Approach 2: Optimized Sliding Window with Two Deques

### Intuition
The brute force solution is inefficient due to redundant checks for the max and min. A more optimal solution can maintain a sliding window of elements, while keeping the current maximum and minimum within this window through deques. As the sliding window expands, the deques are used to update and maintain the max and min efficiently.

### Algorithm
1. Use two deques to store the indices of the elements. One deque stores indices in decreasing order to get the minimum, and another in increasing order for the maximum.
2. Initialize your window's start pointer `left` to zero.
3. Expand the window by moving the `right` pointer and update deques.
4. Check the condition by comparing the max and min using the front of both deques.
5. If the condition is not met (max - min > limit), increment the `left` pointer.
6. Calculate the maximum length of the window that satisfies the condition.
7. Return the result.

### Implementation
```java
import java.util.Deque;
import java.util.LinkedList;

public class Solution {
    public int longestSubarray(int[] nums, int limit) {
        Deque<Integer> maxDeque = new LinkedList<>();
        Deque<Integer> minDeque = new LinkedList<>();
        int left = 0, right;
        int maxLen = 0;
        
        for (right = 0; right < nums.length; right++) {
            // Maintain decreasing order for maxDeque
            while (!maxDeque.isEmpty() && nums[maxDeque.peekLast()] <= nums[right])
                maxDeque.pollLast();
            maxDeque.offerLast(right);

            // Maintain increasing order for minDeque
            while (!minDeque.isEmpty() && nums[minDeque.peekLast()] >= nums[right])
                minDeque.pollLast();
            minDeque.offerLast(right);

            // If the current window doesn't satisfy the condition, slide the window
            while (nums[maxDeque.peekFirst()] - nums[minDeque.peekFirst()] > limit) {
                left++;
                // Remove elements outside the window
                if (maxDeque.peekFirst() < left)
                    maxDeque.pollFirst();
                if (minDeque.peekFirst() < left)
                    minDeque.pollFirst();
            }
            
            // Update max length of satisfying subarray
            maxLen = Math.max(maxLen, right - left + 1);
        }
        
        return maxLen;
    }
}
```

### Time Complexity
- **O(N):** Each element is added and removed from the deque at most once.

### Space Complexity
- **O(N):** Space for the two deques holding indices.

