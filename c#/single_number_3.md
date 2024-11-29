# 260. [Single Number III](https://leetcode.com/problems/single-number-iii/)

## Approach 1: HashMap for Counting Frequencies

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int[] SingleNumber(int[] nums) {
        Dictionary<int, int> countMap = new Dictionary<int, int>();
        
        // Count the frequency of each number
        foreach (int num in nums) {
            if (countMap.ContainsKey(num)) {
                countMap[num]++;
            } else {
                countMap[num] = 1;
            }
        }
        
        int[] result = new int[2];
        int idx = 0;
        // Find the numbers with frequency 1
        foreach (var kvp in countMap) {
            if (kvp.Value == 1) {
                result[idx++] = kvp.Key;
            }
        }
        
        return result;
    }
}
```

## Approach 2: Bit Manipulation

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int[] SingleNumber(int[] nums) {
        int xor = 0;
        
        // XOR all numbers to get xor of the two unique numbers
        foreach (int num in nums) {
            xor ^= num;
        }
        
        // Find a set bit (rightmost) in xor to separate the numbers
        int diff = xor & -xor;
        
        int[] result = new int[2];
        // Separate the numbers into two groups and XOR them respectively
        foreach (int num in nums) {
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

