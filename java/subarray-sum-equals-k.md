# [Leetcode 560: Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approaches

- [Brute Force Approach](#brute-force-approach)
- [Using Cumulative Sum Array](#using-cumulative-sum-array)
- [Using HashMap for Optimized Approach](#using-hashmap-for-optimized-approach)

### Brute Force Approach

The brute force approach checks every possible subarray and sums the elements to check if it equals `k`. This approach is simple to implement but can be inefficient for large arrays.

#### Intuition:

We iterate through each starting point of a subarray and for each starting point, check every possible end point. This gives us every possible subarray.

#### Code:

```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        // Iterate through each starting point of the subarray
        for (int start = 0; start < nums.length; start++) {
            int sum = 0;
            // Iterate through each possible end point
            for (int end = start; end < nums.length; end++) {
                sum += nums[end];  // Add the current number to the sum of the subarray
                if (sum == k) {
                    count++;  // If the sum equals k, we found a subarray
                }
            }
        }
        return count;
    }
}
```

#### Time Complexity:

- **Time:** O(n^2) - We check every subarray which takes a quadratic amount of time.
- **Space:** O(1) - We only use constant space.

### Using Cumulative Sum Array

We can improve our approach by using a cumulative sum array to keep track of the sum of the elements up to each index.

#### Intuition:

The cumulative sum array allows us to quickly compute the sum of elements between two indices in constant time by subtracting the cumulative sum of the index before the start of the subarray from the end index.

#### Code:

```java
public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        int[] cumulativeSum = new int[nums.length + 1];
        cumulativeSum[0] = 0;
        
        // Populate the cumulative sum array
        for (int i = 1; i <= nums.length; i++) {
            cumulativeSum[i] = cumulativeSum[i - 1] + nums[i - 1];
        }
        
        // Check each subarray using the cumulative sum array
        for (int start = 0; start < nums.length; start++) {
            for (int end = start + 1; end <= nums.length; end++) {
                // Sum of nums[start...end-1] is cumulativeSum[end] - cumulativeSum[start]
                if (cumulativeSum[end] - cumulativeSum[start] == k) {
                    count++;
                }
            }
        }
        return count;
    }
}
```

#### Time Complexity:

- **Time:** O(n^2) - Constructing the cumulative sum and checking subarrays is quadratic.
- **Space:** O(n) - We use a cumulative sum array that takes linear space.

### Using HashMap for Optimized Approach

This optimized approach leverages a HashMap to track the number of times a particular cumulative sum has appeared.

#### Intuition:

While iterating through the array, we keep track of the cumulative sum. We use a HashMap to store the frequency of each cumulative sum we have encountered. For each new cumulative sum `currentSum`, we check if `currentSum - k` exists in the HashMap. If it does, it means there is a subarray ending at the current index with sum `k`.

#### Code:

```java
import java.util.HashMap;

public class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        int currentSum = 0;
        HashMap<Integer, Integer> prefixSumCount = new HashMap<>();
        prefixSumCount.put(0, 1);  // Base case: A sum of zero is encountered once

        // Iterate through each number in the array
        for (int num : nums) {
            currentSum += num;  // Update the cumulative sum
            // Check if there is a subarray (ending at current index) which has the sum of k
            if (prefixSumCount.containsKey(currentSum - k)) {
                count += prefixSumCount.get(currentSum - k);
            }
            // Record the current cumulative sum in the hashmap
            prefixSumCount.put(currentSum, prefixSumCount.getOrDefault(currentSum, 0) + 1);
        }
        return count;
    }
}
```

#### Time Complexity:

- **Time:** O(n) - We iterate through the array once while maintaining and querying the HashMap in constant average time.
- **Space:** O(n) - The HashMap can potentially store all cumulative sums encountered.

Overall, the HashMap approach provides the most efficient solution in terms of time complexity while maintaining a manageable space complexity.

