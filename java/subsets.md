# LeetCode Problem - Subsets

[LeetCode Problem 78: Subsets](https://leetcode.com/problems/subsets/)

## Approaches:
- [Backtracking Approach](#backtracking-approach)
- [Iterative Approach](#iterative-approach)
- [Bit Manipulation Approach](#bit-manipulation-approach)

### Backtracking Approach

The backtracking technique is one of the simplest and intuitive ways to solve this problem. The main idea is to generate all possible subsets incrementally. Here's the detailed procedure:

1. Recursion is used to explore inclusion or exclusion of each element in the subset being formed.
2. Maintain a current subset list (path) and we include/exclude elements from nums.
3. On reaching a point where no more elements are left to consider, record the current subset as a valid solution.
4. Start with an empty subset [] and traverse all possible options.

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {

    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(0, nums, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(int start, int[] nums, List<Integer> path, List<List<Integer>> result) {
        // Add the current subset to the result
        result.add(new ArrayList<>(path));
        
        for (int i = start; i < nums.length; i++) {
            // Include nums[i] in the subset
            path.add(nums[i]);
            // Recurse with added element
            backtrack(i + 1, nums, path, result);
            // Backtrack by removing nums[i]
            path.remove(path.size() - 1);
        }
    }

    public static void main(String[] args) {
        Solution sol = new Solution();
        int[] nums = {1, 2, 3};
        List<List<Integer>> subsets = sol.subsets(nums);
        System.out.println(subsets);
    }
}
```

**Time Complexity:** O(2^n) - where `n` is the number of elements in nums. Each element can either be included or not included, leading to 2^n possible subsets.

**Space Complexity:** O(n) - space used by the recursion stack.

### Iterative Approach

The iterative approach builds the subsets directly using the concepts of combinatorial logic, by iteratively adding each element to existing subsets. Here's the thought process:

1. Start with an empty subset.
2. For each number in the array, iterate over all subsets formed so far and include the current number into them to form new subsets.
3. Merge the newly formed subsets into the result.

```java
import java.util.ArrayList;
import java.util.List;

public class IterativeSolution {

    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        // Begin with the empty subset
        result.add(new ArrayList<>());

        for (int num : nums) {
            // For each existing subset, add the current number to create a new subset
            int size = result.size();
            for (int i = 0; i < size; i++) {
                // Create a new subset from an existing one
                List<Integer> newSubset = new ArrayList<>(result.get(i));
                newSubset.add(num);
                result.add(newSubset);
            }
        }

        return result;
    }

    public static void main(String[] args) {
        IterativeSolution sol = new IterativeSolution();
        int[] nums = {1, 2, 3};
        List<List<Integer>> subsets = sol.subsets(nums);
        System.out.println(subsets);
    }
}
```

**Time Complexity:** O(n * 2^n) - We iterate through n elements, and for each element, we potentially double the number of subsets, resulting in a final complexity of n times 2^n subsets.

**Space Complexity:** O(n * 2^n) - Storage for all the subsets.

### Bit Manipulation Approach

This approach uses the concept that each subset can be represented as a binary string of length n, where 1 indicates inclusion and 0 indicates exclusion.

1. There are a total of 2^n subsets possible.
2. Each subset corresponds to the binary representation of numbers from 0 to 2^n - 1.
3. Convert the numbers into binary form, and include elements in the subset based on digit '1' positioning.

```java
import java.util.ArrayList;
import java.util.List;

public class BitManipulationSolution {

    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        int n = nums.length;
        int totalSubsets = 1 << n; // Total = 2^n subsets

        for (int subsetMask = 0; subsetMask < totalSubsets; subsetMask++) {
            List<Integer> subset = new ArrayList<>();
            
            for (int i = 0; i < n; i++) {
                // Check if the i-th bit is set in the subsetMask
                if ((subsetMask & (1 << i)) != 0) {
                    subset.add(nums[i]);
                }
            }
            result.add(subset);
        }

        return result;
    }

    public static void main(String[] args) {
        BitManipulationSolution sol = new BitManipulationSolution();
        int[] nums = {1, 2, 3};
        List<List<Integer>> subsets = sol.subsets(nums);
        System.out.println(subsets);
    }
}
```

**Time Complexity:** O(n * 2^n) - We generate 2^n subsets, and creating each subset takes O(n) time.

**Space Complexity:** O(n * 2^n) - Storage needed for all the subsets.

