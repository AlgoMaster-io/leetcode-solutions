# [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Approach 1: Sliding Window with Deque

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int LongestSubarray(int[] nums, int limit) {
        Deque<int> maxDeque = new Deque<int>(); // Stores indices of max elements in the window
        Deque<int> minDeque = new Deque<int>(); // Stores indices of min elements in the window
        int left = 0, maxLength = 0;

        for (int right = 0; right < nums.Length; right++) {
            // Maintain the decreasing order in maxDeque
            while (maxDeque.Count > 0 && nums[maxDeque.PeekLast()] < nums[right]) {
                maxDeque.RemoveLast();
            }
            maxDeque.AddLast(right);

            // Maintain the increasing order in minDeque
            while (minDeque.Count > 0 && nums[minDeque.PeekLast()] > nums[right]) {
                minDeque.RemoveLast();
            }
            minDeque.AddLast(right);

            // Check if the current window is valid
            while (nums[maxDeque.PeekFirst()] - nums[minDeque.PeekFirst()] > limit) {
                left++; // Shrink the window from the left
                // Remove indices out of the window
                if (maxDeque.PeekFirst() < left) maxDeque.RemoveFirst();
                if (minDeque.PeekFirst() < left) minDeque.RemoveFirst();
            }

            maxLength = Math.Max(maxLength, right - left + 1); // Update the max length
        }

        return maxLength;
    }
}

## Approach 2: Sliding Window with TreeMap

### Solution
c#
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int LongestSubarray(int[] nums, int limit) {
        SortedDictionary<int, int> map = new SortedDictionary<int, int>(); // Stores elements and their frequencies
        int left = 0, maxLength = 0;

        for (int right = 0; right < nums.Length; right++) {
            // Add the current element to the map
            if (!map.ContainsKey(nums[right])) {
                map[nums[right]] = 0;
            }
            map[nums[right]]++;

            // Shrink the window if the condition is violated
            while (map.Keys.Max() - map.Keys.Min() > limit) {
                if (--map[nums[left]] == 0) {
                    map.Remove(nums[left]);
                }
                left++;
            }

            maxLength = Math.Max(maxLength, right - left + 1); // Update the max length
        }

        return maxLength;
    }
}

