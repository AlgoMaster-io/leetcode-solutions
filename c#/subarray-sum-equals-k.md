# [Leetcode 560: Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Cumulative Sum with HashMap](#cumulative-sum-with-hashmap)

### Brute Force Approach

#### Intuition:
The brute force approach involves checking all possible subarrays, calculating their sum, and comparing the sum with `k`. This method is straightforward and involves iterating through all possible starting and ending indices of subarrays.

#### Approach:
1. Iterate over each element as the starting point of a subarray.
2. For each starting point, iterate over each possible ending point.
3. Calculate the sum of the subarray from the starting index to the ending index.
4. If the sum equals `k`, increment the count of such subarrays.

#### Code:

```csharp
public class Solution {
    public int SubarraySum(int[] nums, int k) {
        int count = 0;

        // Iterate through each number as the start of subarray
        for (int start = 0; start < nums.Length; start++) {
            int sum = 0;

            // Iterate through each end index for the current starting point
            for (int end = start; end < nums.Length; end++) {
                sum += nums[end]; // Calculate the sum of the subarray

                // If the current subarray sum equals k, increment the count
                if (sum == k) {
                    count++;
                }
            }
        }

        return count; // Return the total count of subarrays with sum equals k
    }
}
```

#### Time Complexity:
- O(n^2): We are using two nested loops, each can run up to the length of `nums`.

#### Space Complexity:
- O(1): We are using a constant amount of extra space.

### Cumulative Sum with HashMap

#### Intuition:
To optimize the brute force approach, we can use a HashMap to store the cumulative sum up to each index. This allows us to quickly calculate the number of subarrays that result in the desired sum `k` by checking how many times a certain cumulative sum minus `k` has occurred.

#### Approach:
1. Use a HashMap to store the count of each cumulative sum encountered as we traverse the array.
2. Initialize the cumulative sum to 0 and the count of subarrays to 0.
3. As you iterate through the array, update the cumulative sum.
4. Check if `(cumulative sum - k)` exists in the HashMap. If it does, it means there is a subarray ending at the current index whose sum is `k`.
5. Update the count of the cumulative sum in the HashMap.

#### Code:

```csharp
public class Solution {
    public int SubarraySum(int[] nums, int k) {
        int count = 0;
        int cumulativeSum = 0;
        Dictionary<int, int> sumCountMap = new Dictionary<int, int>();
        sumCountMap[0] = 1; // Initialize with sum 0 occurring once for the case when subarray itself starts from index 0

        // Traverse through the array
        foreach (var num in nums) {
            cumulativeSum += num; // Update the cumulative sum

            // Check if there exists a previous cumulative sum such that currentSum - prevSum = k
            if (sumCountMap.ContainsKey(cumulativeSum - k)) {
                count += sumCountMap[cumulativeSum - k]; // Increment count by number of occurrences of that sum
            }

            // Update the occurrence of the current cumulative sum in the map
            if (sumCountMap.ContainsKey(cumulativeSum)) {
                sumCountMap[cumulativeSum]++;
            } else {
                sumCountMap[cumulativeSum] = 1;
            }
        }

        return count; // Return the total count of subarrays with sum equals k
    }
}
```

#### Time Complexity:
- O(n): We traverse the array once while performing constant-time operations for each element.

#### Space Complexity:
- O(n): We use a HashMap to store cumulative sums, in the worst case, we could store all elements.

