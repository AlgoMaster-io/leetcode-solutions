# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        int count = 0;

        // Check all subarrays
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;

            for (int j = i; j < nums.length; j++) {
                sum += nums[j];

                // Check if the sum is divisible by k
                if (sum % k == 0) {
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
// Space Complexity: O(k)
import java.util.HashMap;

public class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        HashMap<Integer, Integer> remainderMap = new HashMap<>();
        remainderMap.put(0, 1); // Initialize with a remainder of 0

        int count = 0;
        int sum = 0;

        for (int num : nums) {
            sum += num;
            int remainder = sum % k;

            // Normalize the remainder to always be non-negative
            if (remainder < 0) {
                remainder += k;
            }

            // If the remainder exists in the map, add its frequency to the count
            if (remainderMap.containsKey(remainder)) {
                count += remainderMap.get(remainder);
            }

            // Update the frequency of the current remainder
            remainderMap.put(remainder, remainderMap.getOrDefault(remainder, 0) + 1);
        }

        return count;
    }
}
```