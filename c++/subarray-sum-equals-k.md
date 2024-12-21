# [Leetcode 560: Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Cumulative Sum and HashMap Approach](#cumulative-sum-and-hashmap-approach)

## Brute Force Approach

### Intuition
The most straightforward approach is to consider all possible subarrays and calculate their sums to check if they are equal to `k`. This involves traversing all subarrays using two nested loops. This solution is simple to implement but inefficient for larger arrays since it involves a lot of repeated work by recalculating sums for overlapping subarrays.

### Code
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int count = 0;
        // Iterate over all possible starting points of subarrays
        for (int start = 0; start < nums.size(); ++start) {
            int sum = 0;
            // Calculate sum for subarrays starting from index 'start'
            for (int end = start; end < nums.size(); ++end) {
                sum += nums[end]; // Accumulate sum from start to end index
                // Check if the current sum equals to k
                if (sum == k) {
                    count++; // Increment count if we found a subarray sum equals to k
                }
            }
        }
        return count;
    }
};
```

### Complexity
- **Time Complexity**: O(n^2) - This is due to the two nested loops used to calculate the sum for subarrays.
- **Space Complexity**: O(1) - No additional space is used apart from local variables.

## Cumulative Sum and HashMap Approach

### Intuition
The brute force approach can be optimized by using a HashMap (unordered_map in C++) to store the cumulative sums and their frequencies. The idea is to keep a running sum of the elements of the array and store the frequency of each running sum in the map. 

For each element, calculate the complement `runningSum - k`. If this complement has been seen before, it indicates that there exists a subarray ending in the current position with a sum equal to `k`. This allows us to efficiently calculate the total number of subarrays for any given prefix of the array.

### Code
```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> sumCount; // Map to store cumulative sum & its frequency
        sumCount[0] = 1; // Initialize to handle the case when subarray starts from index 0
        int runningSum = 0; // Initialize running sum to 0
        int count = 0; // Initialize the count of subarray sums that equal k

        // Iterate through the array
        for (int num : nums) {
            runningSum += num; // Update the running sum by adding the current number

            // Check if there exists a prefix sum that, when subtracted from runningSum, gives k
            if (sumCount.find(runningSum - k) != sumCount.end()) {
                count += sumCount[runningSum - k]; // Increase the count by the number of such prefix sums
            }

            // Increment the frequency of the current running sum in the map
            sumCount[runningSum]++;
        }

        return count; // Return the total count of subarrays with sum equals to k
    }
};
```

### Complexity
- **Time Complexity**: O(n) - We iterate through the array once, and each operation inside the loop is O(1).
- **Space Complexity**: O(n) - In the worst case we could store all unique sums in the map.

