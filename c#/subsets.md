# 78. [Subsets](https://leetcode.com/problems/subsets/)

## Approach 1: Iterative (Power Set)

### Solution
csharp
```csharp
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n * 2^n) for storing the subsets
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> Subsets(int[] nums) {
        List<IList<int>> result = new List<IList<int>>();
        result.Add(new List<int>()); // Start with the empty subset

        foreach (int num in nums) {
            int size = result.Count;
            for (int i = 0; i < size; i++) {
                List<int> subset = new List<int>(result[i]);
                subset.Add(num); // Add the current number to each existing subset
                result.Add(subset);
            }
        }

        return result;
    }
}
```

## Approach 2: Backtracking (Recursive Subset Generation)

### Solution
csharp
```csharp
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n) for the recursion stack
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> Subsets(int[] nums) {
        List<IList<int>> result = new List<IList<int>>();
        Backtrack(nums, 0, new List<int>(), result);
        return result;
    }

    private void Backtrack(int[] nums, int index, List<int> current, List<IList<int>> result) {
        result.Add(new List<int>(current)); // Add the current subset to the result

        for (int i = index; i < nums.Length; i++) {
            current.Add(nums[i]); // Include nums[i] in the current subset
            Backtrack(nums, i + 1, current, result); // Recurse with the next index
            current.RemoveAt(current.Count - 1); // Backtrack to explore other subsets
        }
    }
}
```

## Approach 3: Bitmasking (Iterative Subset Generation)

### Solution
csharp
```csharp
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n * 2^n) for storing the subsets
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> Subsets(int[] nums) {
        List<IList<int>> result = new List<IList<int>>();
        int totalSubsets = 1 << nums.Length; // Total subsets = 2^n

        for (int mask = 0; mask < totalSubsets; mask++) {
            List<int> subset = new List<int>();
            for (int i = 0; i < nums.Length; i++) {
                if ((mask & (1 << i)) != 0) {
                    subset.Add(nums[i]); // Include nums[i] in the subset
                }
            }
            result.Add(subset);
        }

        return result;
    }
}
```

