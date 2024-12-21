# [334. Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized Linear Scan](#approach-2-optimized-linear-scan)

### Approach 1: Brute Force

#### Intuition
The simplest way to find a triplet (i, j, k) such that `nums[i] < nums[j] < nums[k]` is to check all possible combinations of indices (i, j, k). This requires three nested loops to iterate over the array, selecting a pair (i, j) and checking if there exists a valid k.

#### Implementation
```csharp
public class Solution {
    public bool IncreasingTriplet(int[] nums) {
        int n = nums.Length;
        // Iterate over each possible i
        for (int i = 0; i < n - 2; i++) {
            // Iterate over each possible j
            for (int j = i + 1; j < n - 1; j++) {
                // Iterate over each possible k
                for (int k = j + 1; k < n; k++) {
                    // Check if increasing triplet condition is met
                    if (nums[i] < nums[j] && nums[j] < nums[k]) {
                        return true; // A valid triplet is found
                    }
                }
            }
        }
        return false; // No valid triplet found
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n^3), where n is the length of the array. We have three nested loops iterating through the array.
- **Space Complexity**: O(1), no additional space is used.

### Approach 2: Optimized Linear Scan

#### Intuition
The above method is inefficient for large arrays. The task can be accomplished in linear time, O(n), by maintaining two numbers, `first` and `second`, which represent the smallest and second smallest values found while traversing the array. We iterate through each number in the array:
- If the current number is less than or equal to `first`, update `first`.
- Else if the current number is less than or equal to `second`, update `second`.
- Else, if the current number is greater than both, we have found our triplet.

By intelligently tracking potential values for the increasing triplet subsequence, we drastically reduce computational overhead.

#### Implementation
```csharp
public class Solution {
    public bool IncreasingTriplet(int[] nums) {
        int first = int.MaxValue, second = int.MaxValue;
        
        // Traverse each number in the array
        foreach (int num in nums) {
            if (num <= first) {
                // Update first to be the smallest
                first = num;
            } else if (num <= second) {
                // Update second to be the next smallest after first
                second = num;
            } else {
                // If a number greater than both first and second is found, return true
                return true;
            }
        }
        return false; // No triplet found
    }
}
```

#### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the array. We make a single pass through the array.
- **Space Complexity**: O(1), as we are using only a constant amount of extra space.

