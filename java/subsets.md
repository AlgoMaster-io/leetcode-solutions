# 78. [Subsets](https://leetcode.com/problems/subsets/)

## Approach 1: Iterative (Power Set)

### Solution
```java
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n * 2^n) for storing the subsets
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        result.add(new ArrayList<>()); // Start with the empty subset

        for (int num : nums) {
            int size = result.size();
            for (int i = 0; i < size; i++) {
                List<Integer> subset = new ArrayList<>(result.get(i));
                subset.add(num); // Add the current number to each existing subset
                result.add(subset);
            }
        }

        return result;
    }
}
```

## Approach 2: Backtracking (Recursive Subset Generation)

### Solution
```java
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n) for the recursion stack
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(nums, 0, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(int[] nums, int index, List<Integer> current, List<List<Integer>> result) {
        result.add(new ArrayList<>(current)); // Add the current subset to the result

        for (int i = index; i < nums.length; i++) {
            current.add(nums[i]); // Include nums[i] in the current subset
            backtrack(nums, i + 1, current, result); // Recurse with the next index
            current.remove(current.size() - 1); // Backtrack to explore other subsets
        }
    }
}
```

## Approach 3: Bitmasking (Iterative Subset Generation)

### Solution
```java
// Time Complexity: O(n * 2^n), where n is the number of elements
// Space Complexity: O(n * 2^n) for storing the subsets
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        int totalSubsets = 1 << nums.length; // Total subsets = 2^n

        for (int mask = 0; mask < totalSubsets; mask++) {
            List<Integer> subset = new ArrayList<>();
            for (int i = 0; i < nums.length; i++) {
                if ((mask & (1 << i)) != 0) {
                    subset.add(nums[i]); // Include nums[i] in the subset
                }
            }
            result.add(subset);
        }

        return result;
    }
}
```