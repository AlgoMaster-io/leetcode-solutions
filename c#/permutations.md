# 46. [Permutations](https://leetcode.com/problems/permutations/)

## Approach 1: Brute Force (Generate All Permutations)

### Solution
csharp
```csharp
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n * n!) for storing the permutations
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> Permute(int[] nums) {
        var result = new List<IList<int>>();
        var visited = new bool[nums.Length];
        GenerateAll(nums, new List<int>(), result, visited);
        return result;
    }

    private void GenerateAll(int[] nums, List<int> current, List<IList<int>> result, bool[] visited) {
        if (current.Count == nums.Length) {
            result.Add(new List<int>(current)); // Add the current permutation
            return;
        }

        for (int i = 0; i < nums.Length; i++) {
            if (!visited[i]) {
                visited[i] = true;
                current.Add(nums[i]); // Choose the current element
                GenerateAll(nums, current, result, visited); // Recurse
                current.RemoveAt(current.Count - 1); // Backtrack
                visited[i] = false; // Reset visited state
            }
        }
    }
}
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
csharp
```csharp
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n * n!) for storing the permutations
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> Permute(int[] nums) {
        var result = new List<IList<int>>();
        Backtrack(nums, new List<int>(), result);
        return result;
    }

    private void Backtrack(int[] nums, List<int> current, List<IList<int>> result) {
        if (current.Count == nums.Length) {
            result.Add(new List<int>(current)); // Add the current permutation
            return;
        }

        for (int i = 0; i < nums.Length; i++) {
            if (current.Contains(nums[i])) {
                continue; // Skip if the number is already in the current permutation
            }
            current.Add(nums[i]); // Choose the current element
            Backtrack(nums, current, result); // Recurse
            current.RemoveAt(current.Count - 1); // Backtrack
        }
    }
}
```

## Approach 3: Swap-Based Backtracking (In-Place Modification)

### Solution
csharp
```csharp
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n) for the recursion stack
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> Permute(int[] nums) {
        var result = new List<IList<int>>();
        Backtrack(nums, 0, result);
        return result;
    }

    private void Backtrack(int[] nums, int start, List<IList<int>> result) {
        if (start == nums.Length) {
            var current = new List<int>();
            foreach (int num in nums) {
                current.Add(num); // Add the current permutation
            }
            result.Add(current);
            return;
        }

        for (int i = start; i < nums.Length; i++) {
            Swap(nums, start, i); // Swap to create a new permutation
            Backtrack(nums, start + 1, result); // Recurse
            Swap(nums, start, i); // Backtrack to the original state
        }
    }

    private void Swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp; // Swap two elements in the array
    }
}
```

