# 1512. [Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/)

## Approach 1: Brute Force

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int NumIdenticalPairs(int[] nums) {
        int count = 0;

        // Compare every pair of elements
        for (int i = 0; i < nums.Length; i++) {
            for (int j = i + 1; j < nums.Length; j++) {
                if (nums[i] == nums[j]) {
                    count++; // Increment count if pair is "good"
                }
            }
        }

        return count;
    }
}
```

## Approach 2: Dictionary for Counting Frequencies

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int NumIdenticalPairs(int[] nums) {
        Dictionary<int, int> countMap = new Dictionary<int, int>();
        int count = 0;

        // Count the occurrences of each number
        foreach (int num in nums) {
            if (countMap.ContainsKey(num)) {
                count += countMap[num]; // Add the current count of this number
            }
            if (countMap.ContainsKey(num))
                countMap[num]++;
            else
                countMap[num] = 1; // Increment the count
        }

        return count;
    }
}
```

## Approach 3: Frequency Array (Optimized for Small Range)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1) (assuming the range of numbers is limited)
public class Solution {
    public int NumIdenticalPairs(int[] nums) {
        int[] freq = new int[101]; // Frequency array for numbers in range [1, 100]
        int count = 0;

        // Count pairs using frequency
        foreach (int num in nums) {
            count += freq[num]; // Add the number of pairs that can be formed
            freq[num]++; // Increment the frequency of the current number
        }

        return count;
    }
}
```

