# 46. [Permutations](https://leetcode.com/problems/permutations/)

## Approach 1: Brute Force (Generate All Permutations)

### Solution
```java
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n * n!) for storing the permutations
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        boolean[] visited = new boolean[nums.length];
        generateAll(nums, new ArrayList<>(), result, visited);
        return result;
    }

    private void generateAll(int[] nums, List<Integer> current, List<List<Integer>> result, boolean[] visited) {
        if (current.size() == nums.length) {
            result.add(new ArrayList<>(current)); // Add the current permutation
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (!visited[i]) {
                visited[i] = true;
                current.add(nums[i]); // Choose the current element
                generateAll(nums, current, result, visited); // Recurse
                current.remove(current.size() - 1); // Backtrack
                visited[i] = false; // Reset visited state
            }
        }
    }
}
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
```java
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n * n!) for storing the permutations
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(nums, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(int[] nums, List<Integer> current, List<List<Integer>> result) {
        if (current.size() == nums.length) {
            result.add(new ArrayList<>(current)); // Add the current permutation
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (current.contains(nums[i])) {
                continue; // Skip if the number is already in the current permutation
            }
            current.add(nums[i]); // Choose the current element
            backtrack(nums, current, result); // Recurse
            current.remove(current.size() - 1); // Backtrack
        }
    }
}
```

## Approach 3: Swap-Based Backtracking (In-Place Modification)

### Solution
```java
// Time Complexity: O(n * n!), where n is the length of the array
// Space Complexity: O(n) for the recursion stack
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(nums, 0, result);
        return result;
    }

    private void backtrack(int[] nums, int start, List<List<Integer>> result) {
        if (start == nums.length) {
            List<Integer> current = new ArrayList<>();
            for (int num : nums) {
                current.add(num); // Add the current permutation
            }
            result.add(current);
            return;
        }

        for (int i = start; i < nums.length; i++) {
            swap(nums, start, i); // Swap to create a new permutation
            backtrack(nums, start + 1, result); // Recurse
            swap(nums, start, i); // Backtrack to the original state
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp; // Swap two elements in the array
    }
}
```