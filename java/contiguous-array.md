[Leetcode 525: Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Hash Map with Prefix Sum](#approach-2-hash-map-with-prefix-sum)

### Approach 1: Brute Force

#### Intuition
In the brute force approach, we check all possible subarrays to determine if they contain an equal number of 0s and 1s. This can be done by calculating the sum of elements within every possible subarray where each '0' is treated as -1 and '1' as 1. A subarray with a sum of zero has equal numbers of 0s and 1s.

#### Code
```java
public class Solution {
    public int findMaxLength(int[] nums) {
        int maxLength = 0;
        for (int start = 0; start < nums.length; start++) {
            int count = 0;
            for (int end = start; end < nums.length; end++) {
                // Treat 0 as -1, and 1 as 1, calculate the sum
                count += nums[end] == 0 ? -1 : 1;
                // If the count from start to end index is zero, we found an equal subarray
                if (count == 0) {
                    maxLength = Math.max(maxLength, end - start + 1);
                }
            }
        }
        return maxLength;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: \(O(n^2)\) where \(n\) is the number of elements in the array. This is due to checking each possible subarray.
- **Space Complexity**: \(O(1)\) since we are not using any extra space related to input size.

### Approach 2: Hash Map with Prefix Sum

#### Intuition
To improve efficiency, we can use a prefix sum approach combined with a hash map. The key observation is that if two indices have the same prefix sum, the subarray between them has an equal number of 0s and 1s. We maintain a running sum where we treat 0 as -1 and 1 as 1. The hash map stores the first occurrence of each running sum. If at some index the running sum is the same as at a previous index, the subarray between those indices is balanced.

#### Code
```java
import java.util.HashMap;

public class Solution {
    public int findMaxLength(int[] nums) {
        HashMap<Integer, Integer> sumIndexMap = new HashMap<>();
        sumIndexMap.put(0, -1);  // base case to handle entire array sum
        int maxLength = 0;
        int runningSum = 0;
        
        for (int i = 0; i < nums.length; i++) {
            // Update running sum: treat 0 as -1, 1 as 1
            runningSum += nums[i] == 0 ? -1 : 1;
            
            // Check if the running sum has been seen before
            if (sumIndexMap.containsKey(runningSum)) {
                int prevIndex = sumIndexMap.get(runningSum);
                maxLength = Math.max(maxLength, i - prevIndex);
            } else {
                // Store the first occurrence of this running sum
                sumIndexMap.put(runningSum, i);
            }
        }
        
        return maxLength;
    }
}
```

#### Complexity Analysis
- **Time Complexity**: \(O(n)\) where \(n\) is the number of elements in the array. This is due to traversing the array once.
- **Space Complexity**: \(O(n)\) where \(n\) is the number of elements in the array. This is due to the space required for the hash map.

