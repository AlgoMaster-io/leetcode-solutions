# [Leetcode 39: Combination Sum](https://leetcode.com/problems/combination-sum/)

## Approaches
- [Backtracking](#backtracking)
- [Backtracking with Pruning](#backtracking-with-pruning)

### Backtracking

This problem asks us to find all combinations of numbers that can sum up to a target number. We can use a backtracking approach to explore all possible combinations, testing each one to see if it sums to the target.

The backtracking approach works by exploring all potential combinations recursively. For each number in the candidate list, we have a choice to either include it in the current combination or not. If we include it, we subtract it from the target and recursively try to find a new combination that fulfills the reduced target. If we ever reach a negative target, we know the current combination is not viable and backtrack. If we hit zero, we’ve found a valid combination.

#### Steps:
1. Start with an empty combination and the original target.
2. Try every candidate that can contribute to the current combination.
3. Only add to the combination if it doesn't cause the target to go negative.
4. If the target becomes zero, add the current combination as a valid result.
5. Explore further by reusing the same element (since we can use one element multiple times) and recursively backtrack.

```python
def combinationSum(candidates, target):
    def backtrack(start, target, path):
        # Base case: target is met
        if target == 0:
            result.append(list(path))
            return
        
        # Iterate over possible candidates starting from 'start' index
        for i in range(start, len(candidates)):
            candidate = candidates[i]
            # If the candidate exceeds the target, skip it
            if candidate > target:
                continue
            # Include the candidate and explore further
            path.append(candidate)
            # Continue exploration allowing repetitions of the same element
            backtrack(i, target - candidate, path)
            # Backtrack by removing the candidate
            path.pop()

    result = []
    backtrack(0, target, [])
    return result
```
**Time Complexity:** \(O(2^t) \), where \(t\) is the target. This is because, in the worst case, we might explore all subsets of all possible sums of the numbers.

**Space Complexity:** \(O(t) \), where \(t\) is the target, due to the depth of the recursion tree and the space required for the combinations list.

### Backtracking with Pruning

The initial backtracking solution can potentially explore a lot of futile paths. To optimize this, we can sort candidates and skip those paths early where the sum exceeds the target. This pruning cuts down unnecessary explorations, thereby optimizing performance.

#### Steps:
1. Sort the candidates to enable early stopping during the iteration.
2. While adding a candidate to a path, stop further exploration if the candidate exceeds the target.
3. Otherwise, use the candidate and continue recursively as before.

```python
def combinationSum(candidates, target):
    candidates.sort()  # Sort to help with pruning
    def backtrack(start, target, path):
        if target == 0:
            result.append(list(path))
            return

        for i in range(start, len(candidates)):
            candidate = candidates[i]
            if candidate > target:  # Exit early since candidates are sorted
                break
            path.append(candidate)
            backtrack(i, target - candidate, path)
            path.pop()

    result = []
    backtrack(0, target, [])
    return result
```

**Time Complexity:** The sorting takes \(O(n \log n)\) and the backtracking is \(O(2^t)\) similar to the naive backtracking approach. However, due to pruning, practical performance is better.

**Space Complexity:** \(O(t)\) due to recursive depth and storage for the path, similar to the naive approach. 

With sorting and early stopping, this solution is generally faster in practice, particularly for larger input sets, because it avoids unnecessary work.

