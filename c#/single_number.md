# 136. [Single Number](https://leetcode.com/problems/single-number/)

## Approach 1: Using Dictionary

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int SingleNumber(int[] nums) {
        Dictionary<int, int> countMap = new Dictionary<int, int>();

        // Count the frequency of each number
        foreach (int num in nums) {
            if (countMap.ContainsKey(num)) {
                countMap[num]++;
            } else {
                countMap[num] = 1;
            }
        }

        // Find the number with frequency 1
        foreach (int num in nums) {
            if (countMap[num] == 1) {
                return num;
            }
        }

        return -1; // No unique number found
    }
}
```

## Approach 2: Using Sorting

### Solution
csharp
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(1)
using System;

public class Solution {
    public int SingleNumber(int[] nums) {
        Array.Sort(nums); // Sort the array

        // Traverse the array looking for unique element
        for (int i = 0; i < nums.Length - 1; i += 2) {
            if (nums[i] != nums[i + 1]) {
                return nums[i];
            }
        }

        return nums[nums.Length - 1]; // The last element is the single number
    }
}
```

## Approach 3: Using XOR

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int SingleNumber(int[] nums) {
        int result = 0;

        // XOR all elements, duplicate elements will cancel out
        foreach (int num in nums) {
            result ^= num;
        }

        return result; // The remaining single number
    }
}
```

