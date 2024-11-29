# [2461. Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n * k)
// Space Complexity: O(k)
using System;
using System.Collections.Generic;

public class Solution {
    public long MaximumSubarraySum(int[] nums, int k) {
        long maxSum = 0;

        // Iterate over every subarray of size k
        for (int i = 0; i <= nums.Length - k; i++) {
            HashSet<int> uniqueElements = new HashSet<int>();
            long sum = 0;
            bool isValid = true;

            for (int j = i; j < i + k; j++) {
                if (uniqueElements.Contains(nums[j])) {
                    isValid = false; // Subarray contains duplicate
                    break;
                }
                uniqueElements.Add(nums[j]);
                sum += nums[j];
            }

            if (isValid) {
                maxSum = Math.Max(maxSum, sum);
            }
        }

        return maxSum;
    }
}
```

## Approach 2: Sliding Window with HashMap (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(k)
using System;
using System.Collections.Generic;

public class Solution {
    public long MaximumSubarraySum(int[] nums, int k) {
        long maxSum = 0, currentSum = 0;
        Dictionary<int, int> frequencyMap = new Dictionary<int, int>();
        int left = 0;

        // Sliding window
        for (int right = 0; right < nums.Length; right++) {
            currentSum += nums[right];
            if (frequencyMap.ContainsKey(nums[right])) {
                frequencyMap[nums[right]]++;
            } else {
                frequencyMap[nums[right]] = 1;
            }

            // If the window size exceeds k, shrink it
            if (right - left + 1 > k) {
                currentSum -= nums[left];
                frequencyMap[nums[left]]--;
                if (frequencyMap[nums[left]] == 0) {
                    frequencyMap.Remove(nums[left]);
                }
                left++;
            }

            // Check if the current window is valid (distinct elements)
            if (right - left + 1 == k && frequencyMap.Count == k) {
                maxSum = Math.Max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
}
```

