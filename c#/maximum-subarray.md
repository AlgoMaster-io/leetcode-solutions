# [Leetcode 53: Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach - Kadane's Algorithm](#optimized-approach---kadane-s-algorithm)

### Brute Force Approach

In this approach, the idea is to calculate the sum of all possible subarrays in the given array and track the maximum sum encountered.

**Intuition**:  
We iterate over all possible subarrays, compute their sums, and maintain the maximum sum we've found. For an array of size `n`, start with each element and calculate all possible sums up to the end of the array. While this approach guarantees finding the maximum sum, it's not optimal due to its time complexity.

**Steps**:
1. Initialize a variable `max_sum` with a very small value (or `Int32.MinValue`).
2. Use two nested loops to consider every possible subarray.
3. Calculate the sum of the current subarray.
4. Update `max_sum` if the current subarray sum is greater.

**Time Complexity**: O(n^2)  
**Space Complexity**: O(1)  

```csharp
public class Solution {
    public int MaxSubArray(int[] nums) {
        int max_sum = int.MinValue;
        
        // Iterate over each element, treating it as the start of the subarray
        for (int start = 0; start < nums.Length; start++) {
            int current_sum = 0;
            
            // Expand the subarray until the end of the array
            for (int end = start; end < nums.Length; end++) {
                current_sum += nums[end];
                
                // Update max_sum if current_sum is greater
                if (current_sum > max_sum) {
                    max_sum = current_sum;
                }
            }
        }
        
        return max_sum;
    }
}
```

### Optimized Approach - Kadane's Algorithm

Kadane's algorithm improves the performance of finding the maximum subarray sum by making a single pass over the array while maintaining running sums.

**Intuition**:  
The algorithm keeps track of the maximum sum subarray ending at the current position. If the running sum becomes less than zero, it is reset as starting a new subarray because any subarray added to a negative sum would only decrease the sum.

**Steps**:
1. Initialize `current_sum` and `max_sum` with the first element of the array.
2. Traverse the array starting from the second element.
3. Update `current_sum` by taking the maximum of the current element itself and adding the current element to `current_sum`.
4. Update `max_sum` whenever a higher `current_sum` is found.

**Time Complexity**: O(n)  
**Space Complexity**: O(1)  

```csharp
public class Solution {
    public int MaxSubArray(int[] nums) {
        // Initialize current_sum and max_sum with the first element
        int current_sum = nums[0];
        int max_sum = nums[0];
        
        // Traverse the array starting from the second element
        for (int i = 1; i < nums.Length; i++) {
            // Update current_sum to be the maximum between the current element 
            // and the sum of current_sum + current element
            current_sum = Math.Max(nums[i], current_sum + nums[i]);
            
            // Update max_sum if the current_sum is greater
            max_sum = Math.Max(max_sum, current_sum);
        }
        
        return max_sum;
    }
}
```

This optimal solution efficiently finds the maximum subarray sum with just a single traversal of the array, making it suitable for large datasets.

