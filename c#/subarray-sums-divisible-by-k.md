## [Leetcode 974: Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

### Solutions:
- [Brute Force Approach](#brute-force-approach)
- [Prefix Sum with HashMap Approach](#prefix-sum-with-hashmap-approach)

### Brute Force Approach

#### Intuition
The brute force approach involves considering every possible subarray in the given array and checking if the sum of that subarray is divisible by `k`. This can be done by iterating over all the possible starting points of subarrays and for each, iterating over all the possible endpoints to compute the sum of the subarray.

#### Implementation
```csharp
public class Solution {
    public int SubarraysDivByK(int[] nums, int k) {
        int count = 0;
        
        // Iterate over all starting points
        for (int start = 0; start < nums.Length; start++) {
            int sum = 0;
            
            // Iterate over all end points
            for (int end = start; end < nums.Length; end++) {
                sum += nums[end]; // Add element to current subarray sum
                
                // Check if the current sum is divisible by k
                if (sum % k == 0) {
                    count++;
                }
            }
        }
        
        return count; // Return the count of subarrays whose sum is divisible by k
    }
}
```

#### Time Complexity
- **Time Complexity**: \(O(n^2)\), as two nested loops are used to traverse the array.
- **Space Complexity**: \(O(1)\), since no additional space is used except for variables.

### Prefix Sum with HashMap Approach

#### Intuition
To optimize, we can use the prefix sum and a hashmap to store frequency of remainder when a prefix sum is divided by `k`. This takes advantage of the property that if `prefixSum[i] % k == prefixSum[j] % k` then the subarray `(i+1, j)` is divisible by `k`.

#### Implementation
```csharp
public class Solution {
    public int SubarraysDivByK(int[] nums, int k) {
        Dictionary<int, int> remainderCount = new Dictionary<int, int>();
        remainderCount[0] = 1; // To handle subarrays that are directly divisible
        
        int prefixSum = 0;
        int count = 0;
        
        foreach (int num in nums) {
            prefixSum += num; // Add each number to prefix sum
            
            // Find current remainder
            int remainder = prefixSum % k;
            
            // Adjust remainder to handle negative values
            if (remainder < 0) {
                remainder += k;
            }
            
            // If remainder has been seen before, increment count
            if (remainderCount.ContainsKey(remainder)) {
                count += remainderCount[remainder];
            }
            
            // Update remainder count
            if (!remainderCount.ContainsKey(remainder)) {
                remainderCount[remainder] = 0;
            }
            remainderCount[remainder]++;
        }
        
        return count; // Return total count of subarrays divisible by k
    }
}
```

#### Time Complexity
- **Time Complexity**: \(O(n)\), as we traverse the array once and use constant time operations with a hashmap.
- **Space Complexity**: \(O(k)\), due to the storage space used for the hashmap storing remainders. 

By using a hashmap to keep track of the frequencies of remainders, we efficiently reduce the overall time complexity compared to the brute force method.

