# [Leetcode 523: Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Prefix Sum with Modulo HashMap](#prefix-sum-with-modulo-hashmap)

### Brute Force Approach

**Intuition**:  
The brute force approach involves considering all subarrays and calculating their sum to see if it's a multiple of `k`. We can iterate over each start index, then from that start index, iterate through possible end indices, maintaining the sum of the subarray and checking it against `k`.

#### Java Code:
```java
public boolean checkSubarraySum(int[] nums, int k) {
    int n = nums.length;
    // Traverse the array to consider all subarrays
    for (int start = 0; start < n; start++) {
        int sum = 0;
        // Check subarray from 'start' to 'end'
        for (int end = start; end < n; end++) {
            sum += nums[end]; // Add current number to the subarray sum
            // Check whether the sum is a multiple of k (and subarray has at least 2 elements)
            if (end - start > 0 && sum % k == 0) {
                return true;
            }
        }
    }
    return false; // No subarray found with required property
}
```

#### Time Complexity: 
- The time complexity is \(O(n^2)\) due to the nested loops checking all subarrays.
  
#### Space Complexity: 
- The space complexity is \(O(1)\) as only a few integer variables are used.

### Prefix Sum with Modulo HashMap

**Intuition**:  
Instead of checking every subarray, use a hashmap to store the remainder of the prefix sum when divided by `k`. If the same remainder appears again (and the subarray length between these points is greater than 1), it indicates a subarray sum which is a multiple of `k`.

#### Java Code:
```java
import java.util.HashMap;

public boolean checkSubarraySum(int[] nums, int k) {
    // HashMap to store the first occurrence of each modulo value.
    HashMap<Integer, Integer> modMap = new HashMap<>();
    modMap.put(0, -1); // Initial heuristic to handle edge case

    int runningSum = 0;
    for (int i = 0; i < nums.length; i++) {
        runningSum += nums[i]; // Increment the running sum
        int mod = runningSum % k; // Compute modulo with k
        
        // If mod is negative, adjust it to be positive
        if (mod < 0) mod += k;

        // Check if this mod has appeared before
        if (modMap.containsKey(mod)) {
            // Verify the subarray is longer than size 1
            if (i - modMap.get(mod) > 1) {
                return true;
            }
        } else {
            // Store the index of the first time we see this mod
            modMap.put(mod, i);
        }
    }
    return false; // No valid subarray found
}
```

#### Time Complexity: 
- The time complexity is \(O(n)\) as we only traversed the array once.

#### Space Complexity: 
- The space complexity is \(O(min(n, k))\) due to the map storing at most `k` different mod values. In scenarios where many duplicates exist, the space can potentially approach `n`.

