# [Leetcode 1438: Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Sliding Window with Sorted Data Structure](#sliding-window-with-sorted-data-structure)
- [Sliding Window with Monotonic Deques (Optimal)](#sliding-window-with-monotonic-deques-optimal)

---

### Brute Force Approach

#### Intuition
The brute force approach involves checking every possible subarray and calculating the maximum and minimum within each subarray. If the difference between these two values is within the limit, we consider that subarray as a potential candidate for the longest subarray.

#### Implementation

```csharp
public class Solution {
    public int LongestSubarray(int[] nums, int limit) {
        int maxLength = 0;
        
        // Check every subarray starting with i
        for (int i = 0; i < nums.Length; i++) {
            int currentMax = nums[i];
            int currentMin = nums[i];
            
            // Extend the subarray to the end of the array
            for (int j = i; j < nums.Length; j++) {
                currentMax = Math.Max(currentMax, nums[j]);
                currentMin = Math.Min(currentMin, nums[j]);
                
                // If the difference is within the limit, consider it
                if (currentMax - currentMin <= limit) {
                    maxLength = Math.Max(maxLength, j - i + 1);
                } else {
                    break;
                }
            }
        }
        
        return maxLength;
    }
}
```

#### Time Complexity
- **O(n^2)**: For each element, we potentially scan up to the rest of the array, leading to a quadratic scanning.

#### Space Complexity
- **O(1)**: No additional data structures are used apart from variables.

---

### Sliding Window with Sorted Data Structure

#### Intuition
We can use a sliding window technique to traverse the array but maintain two data structures to efficiently get the maximum and minimum values within the current window. The .NET `SortedList` or `SortedDictionary` can help in maintaining the order:

#### Implementation

```csharp
using System.Collections.Generic;

public class Solution {
    public int LongestSubarray(int[] nums, int limit) {
        int left = 0;
        int maxLength = 0;
        
        // SortedDictionary (or SortedList) to keep the current sliding window elements sorted
        var sortedList = new SortedList<int, int>();
        
        for (int right = 0; right < nums.Length; right++) {
            // Add current number to our sorted list/dict
            if (!sortedList.ContainsKey(nums[right])) {
                sortedList[nums[right]] = 0;
            }
            sortedList[nums[right]]++;
            
            // If the current window exceeds the limit, shrink it from the left
            while (sortedList.Keys[sortedList.Count - 1] - sortedList.Keys[0] > limit) {
                sortedList[nums[left]]--;
                if (sortedList[nums[left]] == 0) {
                    sortedList.Remove(nums[left]);
                }
                left++;
            }
            
            // Update the maximum length found
            maxLength = Math.Max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
}
```

#### Time Complexity
- **O(n log n)**: Each insertion and deletion into `SortedList` takes `O(log n)`.

#### Space Complexity
- **O(n)**: Space required by the `SortedList` for storing current window elements.

---

### Sliding Window with Monotonic Deques (Optimal)

#### Intuition
The use of two deques — a "min deque" that keeps track of minimum possible values in the current window and a "max deque" that does the opposite — takes advantage of the fact that each value can only enter and exit the deque once, ensuring the solution remains linear time.

#### Implementation

```csharp
using System.Collections.Generic;

public class Solution {
    public int LongestSubarray(int[] nums, int limit) {
        int left = 0;
        int maxLength = 0;
        
        // Deques to store indexes of useful elements.
        var minDeque = new LinkedList<int>();
        var maxDeque = new LinkedList<int>();
        
        for (int right = 0; right < nums.Length; right++) {
            // Maintain the min deque
            while (minDeque.Count > 0 && nums[minDeque.Last.Value] > nums[right]) {
                minDeque.RemoveLast();
            }
            minDeque.AddLast(right);
            
            // Maintain the max deque
            while (maxDeque.Count > 0 && nums[maxDeque.Last.Value] < nums[right]) {
                maxDeque.RemoveLast();
            }
            maxDeque.AddLast(right);
            
            // Ensure the current window is valid
            while (nums[maxDeque.First.Value] - nums[minDeque.First.Value] > limit) {
                if (minDeque.First.Value == left) minDeque.RemoveFirst();
                if (maxDeque.First.Value == left) maxDeque.RemoveFirst();
                left++;
            }
            
            // Update maximum length
            maxLength = Math.Max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
}
```

#### Time Complexity
- **O(n)**: Each element is pushed and popped from both deques at most once.

#### Space Complexity
- **O(n)**: Space is taken by the deques for current window elements.

---

