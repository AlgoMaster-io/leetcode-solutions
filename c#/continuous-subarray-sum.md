# [Leetcode 523: Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

## Approaches
- [Brute Force](#brute-force)
- [Prefix Sum with HashMap](#prefix-sum-with-hashmap)

### Brute Force

#### Intuition
The brute force approach involves examining all possible subarrays in the given array and checking if any subarray's sum is a multiple of `k`. We iterate over every start point (`i`) and end point (`j`) in the array, compute the sum for subarray `nums[i:j]`, and check if it's divisible by `k`.

#### Code
```csharp
public class Solution {
    public bool CheckSubarraySum(int[] nums, int k) {
        int n = nums.Length;
        
        // Base case: if we have less than 2 numbers, we can't have a sum
        if (n < 2) return false;

        // Try every subarray
        for (int start = 0; start < n - 1; start++) {
            int sum = nums[start];  // Initialize sum with the current starting number.

            for (int end = start + 1; end < n; end++) {
                sum += nums[end];  // Add current end number to sum.

                if (sum % k == 0) { // Check divisibility condition
                    return true;
                }
            }
        }

        return false; // No subarray sum is a multiple of k.
    }
}
```

#### Time and Space Complexity
- **Time Complexity**: O(n^2), where `n` is the length of the input array. This is due to the double loop needed to examine all subarrays.
- **Space Complexity**: O(1), as we use a constant amount of space.

### Prefix Sum with HashMap

#### Intuition
To optimize, we use a prefix sum array and a hash map to store remainders of prefix sums modulo `k`. The idea is that if two prefix sums have the same remainder when divided by `k`, the numbers in between those sums must sum to a multiple of `k`.

1. Initialize a dictionary to store the first occurrence of remainders.
2. Compute the cumulative sum of the array.
3. If the remainder of the current cumulative sum modulo `k` has been seen before, and the difference in their indices is at least 2, then the subarray sums to a multiple of `k`.

#### Code
```csharp
public class Solution {
    public bool CheckSubarraySum(int[] nums, int k) {
        Dictionary<int, int> remainderIndexMap = new Dictionary<int, int>();
        remainderIndexMap.Add(0, -1); // Initialize with remainder 0 at index -1 to handle edge case.
        int cumulativeSum = 0;

        for (int i = 0; i < nums.Length; i++) {
            cumulativeSum += nums[i];  // Update cumulative sum.
            
            // Compute remainder keeping in mind negative numbers.
            int remainder = cumulativeSum % k;
            if (remainder < 0) {
                remainder += k; // Adjust to positive remainder
            }

            // Check if this remainder was seen before
            if (remainderIndexMap.ContainsKey(remainder)) {
                // Check if subarray size is at least 2
                if (i - remainderIndexMap[remainder] > 1) {
                    return true;
                }
            } else {
                // Store the first occurrence of this remainder
                remainderIndexMap[remainder] = i;
            }
        }

        return false; // No suitable subarray found
    }
}
```

#### Time and Space Complexity
- **Time Complexity**: O(n), where `n` is the length of the input array. We only pass through the array once.
- **Space Complexity**: O(min(n, k)), for the hash map storing the remainders, though k generally makes it O(k).

This approach efficiently reduces unnecessary recalculations and checks vast numbers of subarray combinations by leveraging modular arithmetic principles.

