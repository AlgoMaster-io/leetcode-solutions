# [525. Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int findMaxLength(int[] nums) {
        int maxLength = 0;

        // Check all possible subarrays
        for (int i = 0; i < nums.length; i++) {
            int count = 0;

            for (int j = i; j < nums.length; j++) {
                // Increment or decrement count based on value
                count += nums[j] == 1 ? 1 : -1;

                // If count is zero, it means equal number of 0s and 1s
                if (count == 0) {
                    maxLength = Math.max(maxLength, j - i + 1);
                }
            }
        }

        return maxLength;
    }
}
```

## Approach 2: HashMap (Optimal)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public int findMaxLength(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, -1); // Initialize with a count of 0 at index -1
        int maxLength = 0;
        int count = 0;

        for (int i = 0; i < nums.length; i++) {
            // Increment or decrement count based on value
            count += nums[i] == 1 ? 1 : -1;

            if (map.containsKey(count)) {
                // Calculate the length of the subarray
                maxLength = Math.max(maxLength, i - map.get(count));
            } else {
                // Store the first occurrence of this count
                map.put(count, i);
            }
        }

        return maxLength;
    }
}
```