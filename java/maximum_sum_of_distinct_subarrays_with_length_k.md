# [2461. Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n * k)
// Space Complexity: O(k)
import java.util.*;

public class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        long maxSum = 0;

        // Iterate over every subarray of size k
        for (int i = 0; i <= nums.length - k; i++) {
            Set<Integer> uniqueElements = new HashSet<>();
            long sum = 0;
            boolean isValid = true;

            for (int j = i; j < i + k; j++) {
                if (uniqueElements.contains(nums[j])) {
                    isValid = false; // Subarray contains duplicate
                    break;
                }
                uniqueElements.add(nums[j]);
                sum += nums[j];
            }

            if (isValid) {
                maxSum = Math.max(maxSum, sum);
            }
        }

        return maxSum;
    }
}
```

## Approach 2: Sliding Window with HashMap (Optimal)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(k)
import java.util.*;

public class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        long maxSum = 0, currentSum = 0;
        Map<Integer, Integer> frequencyMap = new HashMap<>();
        int left = 0;

        // Sliding window
        for (int right = 0; right < nums.length; right++) {
            currentSum += nums[right];
            frequencyMap.put(nums[right], frequencyMap.getOrDefault(nums[right], 0) + 1);

            // If the window size exceeds k, shrink it
            if (right - left + 1 > k) {
                currentSum -= nums[left];
                frequencyMap.put(nums[left], frequencyMap.get(nums[left]) - 1);
                if (frequencyMap.get(nums[left]) == 0) {
                    frequencyMap.remove(nums[left]);
                }
                left++;
            }

            // Check if the current window is valid (distinct elements)
            if (right - left + 1 == k && frequencyMap.size() == k) {
                maxSum = Math.max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
}
```