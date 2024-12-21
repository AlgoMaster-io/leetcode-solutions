# [Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k)

## Solutions
- [Brute Force Approach](#brute-force-approach)
- [Sliding Window Approach](#sliding-window-approach)

### Brute Force Approach

#### Intuition
The brute force approach involves evaluating every possible subarray of length `k`, checking if all the elements within the subarray are distinct, and then calculating the sum. We can then keep track of the maximum sum encountered with the valid distinct subarray.

#### Steps
1. Iterate over every possible starting index for a subarray of length `k`.
2. For each starting index, form the subarray and check if it contains distinct elements.
3. If the subarray is valid, calculate its sum and update the maximum sum encountered.
4. Finally, return the maximum sum found.

#### Time Complexity
- **O(n * k)**: Where `n` is the total number of elements in the array. For each possible starting position, we need to verify if the subarray is distinct, which takes O(k) time.

#### Space Complexity
- **O(k)**: For storing elements within a subarray to verify distinctness.

```csharp
public class Solution {
    public int MaximumSumOfDistinctSubarraysWithK(int[] nums, int k) {
        int maxSum = 0;
        for (int i = 0; i <= nums.Length - k; i++) {
            HashSet<int> set = new HashSet<int>();
            int currentSum = 0;
            bool allDistinct = true;
            // Check subarray from i to i+k
            for (int j = i; j < i + k; j++) {
                if (set.Contains(nums[j])) {
                    allDistinct = false; 
                    break; // If not distinct, skip
                }
                set.Add(nums[j]);
                currentSum += nums[j];
            }
            if (allDistinct) {
                maxSum = Math.Max(maxSum, currentSum);
            }
        }
        return maxSum;
    }
}
```

### Sliding Window Approach

#### Intuition
The sliding window technique can improve efficiency by maintaining a window of length `k` while shifting it across the array. The idea is to expand the window to the right, and adjust the window size when encountering duplicate elements until all elements within the window are distinct. This allows a more efficient scan of the array using hash tables to track element occurrences.

#### Steps
1. Use a sliding window with two pointers, `start` and `end`.
2. Use a dictionary to count occurrences of each element in the current window.
3. Expand the `end` pointer to include new elements and move the `start` pointer to ensure all elements in the window are distinct.
4. Calculate and update the maximum sum if the window size is exactly `k`.
5. Return the maximum sum found.

#### Time Complexity
- **O(n)**: Each element is added and removed from the window at most once, resulting in linear time complexity.

#### Space Complexity
- **O(k)**: Hash table to store current window element counts.

```csharp
public class Solution {
    public int MaximumSumOfDistinctSubarraysWithK(int[] nums, int k) {
        Dictionary<int, int> elementCount = new Dictionary<int, int>();
        int start = 0;
        int currentSum = 0;
        int maxSum = 0;

        for (int end = 0; end < nums.Length; end++) {
            if (!elementCount.ContainsKey(nums[end])) {
                elementCount[nums[end]] = 0;
            }
            elementCount[nums[end]]++;
            currentSum += nums[end];

            // If window size is greater than k, shrink from start
            while (end - start + 1 > k) {
                currentSum -= nums[start];
                elementCount[nums[start]]--;
                if (elementCount[nums[start]] == 0) {
                    elementCount.Remove(nums[start]);
                }
                start++;
            }
            
            // If the window size is exactly k, check if all elements are distinct
            if (end - start + 1 == k && elementCount.Count == k) {
                maxSum = Math.Max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
}
```
This solution efficiently tracks distinct elements within the window and adjusts the window size using a sliding approach, ensuring the maximum sum of distinct subarrays is found in optimal time.

