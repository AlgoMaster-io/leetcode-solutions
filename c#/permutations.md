# LeetCode Problem: [46. Permutations](https://leetcode.com/problems/permutations/)

## Approaches:
- [Backtracking Approach](#backtracking-approach)
- [Iterative Permutation Generation](#iterative-permutation-generation)

## Backtracking Approach

### Intuition
The basic idea behind solving the permutations problem is to generate all possible arrangements of a given list of distinct numbers. A common approach to generating all permutations is using backtracking, a technique often used to solve constraint satisfaction problems. The idea is to build the permutation one element at a time, and make use of recursive calls to explore further possibilities. By swapping elements, we can generate new permutations.

### Algorithm
1. If we have a single element, it's already a permutation.
2. For each element:
   - Swap the element with the starting element.
   - Recursively compute all permutations for the remaining elements.
   - Swap back to restore the original list state (backtrack).

### Code

```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> Permute(int[] nums) {
        var result = new List<IList<int>>();
        Backtrack(nums, 0, result);
        return result;
    }
    
    private void Backtrack(int[] nums, int start, IList<IList<int>> result) {
        if (start == nums.Length) {
            // When we've reached the end of the array, add the permutation to the results.
            result.Add(new List<int>(nums));
            return;
        }
        
        for (int i = start; i < nums.Length; i++) {
            // Swap element i with start.
            Swap(nums, start, i);
            // Recurse with the next starting index.
            Backtrack(nums, start + 1, result);
            // Swap back to restore original array (backtrack).
            Swap(nums, start, i);
        }
    }
    
    private void Swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

### Time Complexity
- **Time:** O(n * n!), where n is the number of elements in the input list. There are n! permutations and generating each takes O(n) work.
- **Space:** O(n), excluding the space for output storage. The recursion depth can go up to n.

## Iterative Permutation Generation

### Intuition
Instead of using recursion, we can generate permutations iteratively by starting with an empty permutation and gradually adding elements in all possible positions. This approach leverages a queue-style method where permutations of increasing sizes are built step-by-step.

### Algorithm
1. Start with an initial list containing an empty permutation.
2. For each number, take the existing permutations and add the number in every possible position.
3. Repeat until every number has been inserted.

### Code

```csharp
using System;
using System.Collections.Generic;

public class Solution {
    public IList<IList<int>> Permute(int[] nums) {
        var result = new List<IList<int>>();
        
        // Start with an empty permutation.
        result.Add(new List<int>());
        
        foreach (int num in nums) {
            var newResult = new List<IList<int>>();
            
            foreach (var perm in result) {
                for (int i = 0; i <= perm.Count; i++) {
                    var newPerm = new List<int>(perm);
                    // Insert the current number into every possible position.
                    newPerm.Insert(i, num);
                    newResult.Add(newPerm);
                }
            }
            
            // Move to the new list with the added number.
            result = newResult;
        }
        
        return result;
    }
}
```

### Time Complexity
- **Time:** O(n * n!), due to the number of permutations being generated.
- **Space:** O(n * n!), as we store all the permutations explicitly. 

In conclusion, both approaches efficiently generate all permutations, with the backtracking solution being memory efficient over iterative but requiring a thought on recursive tree unwinding.

