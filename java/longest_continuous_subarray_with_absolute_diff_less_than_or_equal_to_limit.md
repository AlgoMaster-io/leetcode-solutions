# [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Approach 1: Sliding Window with Deque

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.Deque;
import java.util.LinkedList;

public class Solution {
    public int longestSubarray(int[] nums, int limit) {
        Deque<Integer> maxDeque = new LinkedList<>(); // Stores indices of max elements in the window
        Deque<Integer> minDeque = new LinkedList<>(); // Stores indices of min elements in the window
        int left = 0, maxLength = 0;

        for (int right = 0; right < nums.length; right++) {
            // Maintain the decreasing order in maxDeque
            while (!maxDeque.isEmpty() && nums[maxDeque.peekLast()] < nums[right]) {
                maxDeque.pollLast();
            }
            maxDeque.offerLast(right);

            // Maintain the increasing order in minDeque
            while (!minDeque.isEmpty() && nums[minDeque.peekLast()] > nums[right]) {
                minDeque.pollLast();
            }
            minDeque.offerLast(right);

            // Check if the current window is valid
            while (nums[maxDeque.peekFirst()] - nums[minDeque.peekFirst()] > limit) {
                left++; // Shrink the window from the left
                // Remove indices out of the window
                if (maxDeque.peekFirst() < left) maxDeque.pollFirst();
                if (minDeque.peekFirst() < left) minDeque.pollFirst();
            }

            maxLength = Math.max(maxLength, right - left + 1); // Update the max length
        }

        return maxLength;
    }
}
```

## Approach 2: Sliding Window with TreeMap

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import java.util.TreeMap;

public class Solution {
    public int longestSubarray(int[] nums, int limit) {
        TreeMap<Integer, Integer> map = new TreeMap<>(); // Stores elements and their frequencies
        int left = 0, maxLength = 0;

        for (int right = 0; right < nums.length; right++) {
            // Add the current element to the map
            map.put(nums[right], map.getOrDefault(nums[right], 0) + 1);

            // Shrink the window if the condition is violated
            while (map.lastKey() - map.firstKey() > limit) {
                map.put(nums[left], map.get(nums[left]) - 1);
                if (map.get(nums[left]) == 0) {
                    map.remove(nums[left]);
                }
                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1); // Update the max length
        }

        return maxLength;
    }
}
```