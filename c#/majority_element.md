# [169. Majority Element](https://leetcode.com/problems/majority-element/)

## Approach 1: Brute Force

### Solution
c#
// Time Complexity: O(n^2)
// Space Complexity: O(1)
```csharp
public class Solution {
    public int MajorityElement(int[] nums) {
        int majorityCount = nums.Length / 2;

        // Check the count of each element
        foreach (int num in nums) {
            int count = 0;
            foreach (int element in nums) {
                if (element == num) {
                    count++;
                }
            }
            // If count exceeds majority, return the element
            if (count > majorityCount) {
                return num;
            }
        }

        return -1; // Shouldn't reach here as per problem constraints
    }
}
```

## Approach 2: Dictionary to Count Frequencies

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(n)
```csharp
using System.Collections.Generic;

public class Solution {
    public int MajorityElement(int[] nums) {
        Dictionary<int, int> countMap = new Dictionary<int, int>();
        int majorityCount = nums.Length / 2;

        // Count frequencies of elements
        foreach (int num in nums) {
            if (countMap.ContainsKey(num)) {
                countMap[num]++;
            } else {
                countMap[num] = 1;
            }

            // If an element's count exceeds majority, return it
            if (countMap[num] > majorityCount) {
                return num;
            }
        }

        return -1; // Shouldn't reach here as per problem constraints
    }
}
```

## Approach 3: Sorting

### Solution
c#
// Time Complexity: O(n log n)
// Space Complexity: O(1)
```csharp
using System;

public class Solution {
    public int MajorityElement(int[] nums) {
        // Sort the array
        Array.Sort(nums);

        // The majority element will always be at the middle index
        return nums[nums.Length / 2];
    }
}
```

## Approach 4: Boyer-Moore Voting Algorithm

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(1)
```csharp
public class Solution {
    public int MajorityElement(int[] nums) {
        int count = 0;
        int? candidate = null;

        // Find the candidate for the majority element
        foreach (int num in nums) {
            if (count == 0) {
                candidate = num; // Set a new candidate
            }
            count += (num == candidate) ? 1 : -1;
        }

        return candidate.Value; // The problem guarantees the majority element exists
    }
}
```

