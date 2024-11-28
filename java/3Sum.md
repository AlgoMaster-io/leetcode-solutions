# [15. 3Sum](https://leetcode.com/problems/3sum/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^3)
// Space Complexity: O(1) (excluding output list)
import java.util.*;

public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> result = new HashSet<>();
        int n = nums.length;

        // Iterate through all possible triplets
        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> triplet = Arrays.asList(nums[i], nums[j], nums[k]);
                        Collections.sort(triplet); // Ensure the triplet is in sorted order
                        result.add(triplet); // Add triplet to the set to avoid duplicates
                    }
                }
            }
        }

        return new ArrayList<>(result); // Convert set to list for the output
    }
}
```

## Approach 2: Two Pointers (Optimal)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n) (for sorting or output list)
import java.util.*;

public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums); // Sort the array to use two-pointer technique

        for (int i = 0; i < nums.length - 2; i++) {
            // Skip duplicates for the first element
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;

            // Two-pointer approach
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == 0) {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));

                    // Skip duplicates for the second and third elements
                    while (left < right && nums[left] == nums[left + 1]) left++;
                    while (left < right && nums[right] == nums[right - 1]) right--;

                    left++;
                    right--;
                } else if (sum < 0) {
                    left++; // Increase the sum
                } else {
                    right--; // Decrease the sum
                }
            }
        }

        return result;
    }
}
```