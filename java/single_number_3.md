# 260. [Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approach 1: HashMap for Counting Frequencies

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.HashMap;

public class Solution {
    public int[] singleNumber(int[] nums) {
        HashMap<Integer, Integer> countMap = new HashMap<>();
        
        // Count the frequency of each number
        for (int num : nums) {
            countMap.put(num, countMap.getOrDefault(num, 0) + 1);
        }
        
        int[] result = new int[2];
        int idx = 0;
        // Find the numbers with frequency 1
        for (int num : countMap.keySet()) {
            if (countMap.get(num) == 1) {
                result[idx++] = num;
            }
        }
        
        return result;
    }
}
```

## Approach 2: Bit Manipulation

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int[] singleNumber(int[] nums) {
        int xor = 0;
        
        // XOR all numbers to get xor of the two unique numbers
        for (int num : nums) {
            xor ^= num;
        }
        
        // Find a set bit (rightmost) in xor to separate the numbers
        int diff = xor & -xor;
        
        int[] result = new int[2];
        // Separate the numbers into two groups and XOR them respectively
        for (int num : nums) {
            if ((num & diff) == 0) {
                result[0] ^= num; // XOR for first group
            } else {
                result[1] ^= num; // XOR for second group
            }
        }
        
        return result;
    }
}
```

