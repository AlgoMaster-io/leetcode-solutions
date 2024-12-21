# [Leetcode 78: Subsets](https://leetcode.com/problems/subsets/)

In this problem, we are given a set of distinct integers, nums, and we need to return all possible subsets (the power set).

Given that there are multiple approaches to solving this problem, we will explore them from the most simple and intuitive to more optimal solutions.

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Backtracking Approach](#backtracking-approach)
3. [Iterative Approach](#iterative-approach)
4. [Bit Manipulation Approach](#bit-manipulation-approach)

---

### Recursive Approach

The first approach uses recursion to explore each subset possibility. We branch out at each recursive step, deciding for each element whether to include it in the current subset or not.

#### Intuition:
- For each element, we have two choices: include it or exclude it in the current subset.
- Start from an empty subset and recurse over the elements to decide their inclusion.

```csharp
public class Solution {
    public IList<IList<int>> Subsets(int[] nums) {
        IList<IList<int>> result = new List<IList<int>>();
        GenerateSubsets(0, nums, new List<int>(), result);
        return result;
    }

    private void GenerateSubsets(int index, int[] nums, List<int> current, IList<IList<int>> result) {
        // Add the current subset to the final results list
        result.Add(new List<int>(current));

        for (int i = index; i < nums.Length; i++) {
            // Include the number in the subset
            current.Add(nums[i]);
            // Recurse with the next numbers
            GenerateSubsets(i + 1, nums, current, result);
            // Backtrack and remove the number from the subset
            current.RemoveAt(current.Count - 1);
        }
    }
}
```

**Time Complexity:** O(2^n * n), where n is the number of numbers in `nums`. Each subset costs O(n) to record.

**Space Complexity:** O(n), the recursion stack depth.

---

### Backtracking Approach

We improve upon the recursive approach by systematically searching for the combination of numbers and utilizing backtracking to reduce unnecessary computations. This method involves deciding whether to include each element in the subset.

#### Intuition:
- Similar to the recursive approach but leverages a more structured decision tree to build subsets.

```csharp
public class Solution {
    public IList<IList<int>> Subsets(int[] nums) {
        IList<IList<int>> result = new List<IList<int>>();
        Backtrack(0, nums, new List<int>(), result);
        return result;
    }

    private void Backtrack(int index, int[] nums, List<int> current, IList<IList<int>> result) {
        // Add the current subset to results
        result.Add(new List<int>(current));

        for (int i = index; i < nums.Length; i++) {
            // Decision to include nums[i]
            current.Add(nums[i]);
            // Recursively build subsets including nums[i]
            Backtrack(i + 1, nums, current, result);
            // Undo the decision (backtrack)
            current.RemoveAt(current.Count - 1);
        }
    }
}
```

**Time Complexity:** O(2^n * n), where n is the length of `nums`.

**Space Complexity:** O(n), considering the recursion stack used.

---

### Iterative Approach

This approach iteratively constructs subsets by sequentially adding each new element to all existing subsets to form new subsets.

#### Intuition:
- Start with an empty subset and for each element in `nums`, form new subsets by adding the element to all current subsets.

```csharp
public class Solution {
    public IList<IList<int>> Subsets(int[] nums) {
        IList<IList<int>> result = new List<IList<int>>();
        result.Add(new List<int>());

        foreach (int num in nums) {
            int size = result.Count;
            for (int i = 0; i < size; i++) {
                List<int> newSubset = new List<int>(result[i]);
                newSubset.Add(num);
                result.Add(newSubset);
            }
        }

        return result;
    }
}
```

**Time Complexity:** O(2^n * n), since subsets double as we add more numbers.

**Space Complexity:** O(2^n * n), needed to store all the subsets.

---

### Bit Manipulation Approach

This approach leverages the concept of binary numbers to generate all possible subsets, treating each bit of the number as a decision to include a particular element or not.

#### Intuition:
- Each subset can be represented as a sequence of binary digits, where '1' means include the element, and '0' means exclude it.
- Generate all 2^n possible binary numbers from 0 to 2^n - 1 and map them to subsets.

```csharp
public class Solution {
    public IList<IList<int>> Subsets(int[] nums) {
        int subsetCount = 1 << nums.Length; // 2^n
        IList<IList<int>> result = new List<IList<int>>();

        for (int i = 0; i < subsetCount; i++) {
            List<int> subset = new List<int>();
            for (int j = 0; j < nums.Length; j++) {
                // Check if the jth bit of i is set
                if ((i & (1 << j)) != 0) {
                    subset.Add(nums[j]);
                }
            }
            result.Add(subset);
        }

        return result;
    }
}
```

**Time Complexity:** O(2^n * n), as we have 2^n subsets and copying each subset takes O(n).

**Space Complexity:** O(2^n * n), for storing all subsets.

Each approach has its own merits, with the bit manipulation approach being particularly elegant in its use of binary number representation. Choose the one that best fits the constraints and environment in which you're working.

