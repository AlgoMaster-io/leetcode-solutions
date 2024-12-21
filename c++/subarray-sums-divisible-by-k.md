## [Leetcode 974: Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimal Approach using Hash Map](#optimal-approach-using-hash-map)

### Brute Force Approach

The brute force approach involves going through all possible subarrays and checking if their sum is divisible by `K`. This method has a higher time complexity since it checks the sum of each subarray individually.

#### Intuition
The idea is to consider every possible subarray of the given array and calculate its sum. If the sum is divisible by `K`, increment the count accordingly. This approach simply uses nested loops to explore all subarray possibilities.

#### Complexity Analysis
- **Time Complexity**: O(N^2), where N is the number of elements in the array. We need two nested loops to iterate through the array to explore every subarray.
- **Space Complexity**: O(1), because we do not use any extra space which scales with input size.

```cpp
#include <vector>

int subarraysDivByK(std::vector<int>& nums, int K) {
    int n = nums.size();
    int count = 0;
    
    // Consider every subarray starting with i
    for (int i = 0; i < n; ++i) {
        int sum = 0;
        // Consider every subarray ending with j
        for (int j = i; j < n; ++j) {
            sum += nums[j];
            // Check if the current sum is divisible by K
            if (sum % K == 0) {
                ++count;
            }
        }
    }
    
    return count;
}
```

### Optimal Approach using Hash Map

This optimal approach uses a hash map to store the frequency of remainder values when the cumulative sum of elements is divided by `K`. This method efficiently checks for subarrays with sum divisible by `K` using the prefix sum concept.

#### Intuition
The key observation is using prefix sums and understanding that if `(prefix[j] - prefix[i]) % K == 0`, it implies `prefix[j] % K == prefix[i] % K`. We use a hash map to store the frequency of each remainder encountered and use it to find the count of divisible subarrays.

#### Steps:
1. Initialize a hash map to store the frequencies of remainders.
2. Start with a cumulative sum of 0, with an initial entry for a remainder of 0 in the map (0 remainder occurs once initially).
3. Traverse the array:
   - Update the cumulative sum.
   - Compute the remainder `((cumulative sum % K) + K) % K` to handle negative cases.
   - If this remainder has been seen before, it means subarrays adding up to a multiple of `K` exist between previous occurrences of the same remainder.
   - Update the count by the frequency of the current remainder.
   - Update the frequency of the current remainder in the hash map.

#### Complexity Analysis
- **Time Complexity**: O(N), where N is the number of elements in the array, as we traverse the array once.
- **Space Complexity**: O(K), because we store at most K distinct remainders in the hash map.

```cpp
#include <vector>
#include <unordered_map>

int subarraysDivByK(std::vector<int>& nums, int K) {
    std::unordered_map<int, int> remainderMap;
    remainderMap[0] = 1;  // base case for prefix sum yielding exactly divisible
    int cumulativeSum = 0;
    int count = 0;
    
    for (int num : nums) {
        cumulativeSum += num;
        int remainder = ((cumulativeSum % K) + K) % K;  // handle negative remainders
        // If the remainder exists in the map, there are subarrays summing to a multiple of K
        if (remainderMap.find(remainder) != remainderMap.end()) {
            count += remainderMap[remainder];
        }
        
        // Update the frequency of the current remainder
        ++remainderMap[remainder];
    }
    
    return count;
}
```

This optimal solution efficiently finds the count of subarrays whose sum is divisible by `K` using prefix sums and a hash map.

