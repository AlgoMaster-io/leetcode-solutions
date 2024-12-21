[Problem: Leetcode 40: Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

### Approaches:
- [Approach 1: Recursive Backtracking with Duplicate Checking](#approach-1)
- [Approach 2: Recursive Backtracking with Early Stop and Pruning](#approach-2)

## Approach 1: Recursive Backtracking with Duplicate Checking

### Intuition:
The core idea is to use backtracking to explore all potential combinations. For each number, decide to either include it in the combination or skip it, but be cautious to avoid duplicates by skipping subsequent numbers that have already been tried at the current position.

### Detailed Steps:
1. Sort the `candidates` array; this helps in easily identifying duplicates.
2. Use a backtracking function that explores possibilities:
   - If the target becomes zero, add the current combination to the result as it sums up to the desired target.
   - For each candidate starting at the current position, choose it and recurse to check further numbers.
   - If a number has the same value as a previously checked number at the same tree depth, skip it to avoid creating duplicates.

### Code:

```python
def combinationSum2(candidates, target):
    def backtrack(start, path, target):
        if target == 0:
            result.append(list(path))
            return
        elif target < 0:
            return
        
        for i in range(start, len(candidates)):
            # Skip duplicates
            if i > start and candidates[i] == candidates[i - 1]:
                continue
            
            path.append(candidates[i])  # Choose the number
            backtrack(i + 1, path, target - candidates[i])  # Recur
            path.pop()  # Backtrack, remove chosen number
    
    candidates.sort()  # Sort to handle duplicates
    result = []
    backtrack(0, [], target)
    
    return result
```

### Complexity Analysis:
- **Time Complexity:** \(O(2^n)\), where \(n\) is the number of candidates. We explore each subset.
- **Space Complexity:** \(O(n)\), for the recursion stack and storing the current path.

## Approach 2: Recursive Backtracking with Early Stop and Pruning

### Intuition:
Enhancing the first approach, we can further optimize by pruning the search tree whenever we realize that summing up further numbers will not help us reach the target, i.e., as soon as the remaining numbers exceed the target.

### Detailed Steps:
1. Sort the `candidates` array to facilitate easy duplicate handling and pruning.
2. Implement the backtracking:
   - As soon as the `target` becomes negative, stop further processing (early stop).
   - As soon as a candidate number itself is greater than the remaining target, stop the loop since remaining numbers are also larger due to sorting.

### Code:

```python
def combinationSum2(candidates, target):
    def backtrack(start, path, target):
        if target == 0:
            result.append(list(path))
            return
        elif target < 0:
            return
        
        for i in range(start, len(candidates)):
            # Skip duplicates
            if i > start and candidates[i] == candidates[i - 1]:
                continue
            
            # Prune deeper searches where candidates[i] > target
            if candidates[i] > target:
                break
            
            path.append(candidates[i])  # Choose the number
            backtrack(i + 1, path, target - candidates[i])  # Recur
            path.pop()  # Backtrack
    
    candidates.sort()  # Sort the array to allow pruning
    result = []
    backtrack(0, [], target)
    
    return result
```

### Complexity Analysis:
- **Time Complexity:** \(O(2^n)\), similar to the first approach, though practical execution is faster due to pruning.
- **Space Complexity:** \(O(n)\), for recursion stack and storing the current path.

In conclusion, by applying sorting and intelligent skipping, combined with pruning, we can solve the problem efficiently while avoiding duplicates.

