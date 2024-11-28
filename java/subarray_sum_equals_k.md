# [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;

        // Check all subarrays
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;

            for (int j = i; j < nums.length; j++) {
                sum += nums[j];

                // If the sum equals k, increment the count
                if (sum == k) {
                    count++;
                }
            }
        }

        return count;
    }
}
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public int subarraySum(int[] nums, int k) {
        HashMap<Integer, Integer> prefixSumMap = new HashMap<>();
        prefixSumMap.put(0, 1); // Initialize with a prefix sum of 0

        int count = 0;
        int sum = 0;

        for (int num : nums) {
            sum += num; // Update the prefix sum

            // Check if there's a prefix sum that makes the current sum equal to k
            if (prefixSumMap.containsKey(sum - k)) {
                count += prefixSumMap.get(sum - k);
            }

            // Update the frequency of the current prefix sum
            prefixSumMap.put(sum, prefixSumMap.getOrDefault(sum, 0) + 1);
        }

        return count;
    }
}
```