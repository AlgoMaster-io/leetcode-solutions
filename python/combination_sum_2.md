# 40. [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

## Approach 1: Backtracking with Duplicate Handling

### Solution
python
```python
# Time Complexity: O(2^n), where n is the number of candidates
# Space Complexity: O(k), where k is the average length of a combination
from typing import List

class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        candidates.sort()  # Sort the candidates to handle duplicates
        self.backtrack(candidates, target, 0, [], result)
        return result

    def backtrack(self, candidates: List[int], target: int, start: int, current: List[int], result: List[List[int]]):
        if target == 0:
            result.append(list(current))  # Add the valid combination to the result
            return

        for i in range(start, len(candidates)):
            if i > start and candidates[i] == candidates[i - 1]:
                continue  # Skip duplicates
            if candidates[i] > target:
                break  # Stop the loop if the current candidate exceeds the target

            current.append(candidates[i])  # Choose the current candidate
            self.backtrack(candidates, target - candidates[i], i + 1, current, result)  # Recurse with next index
            current.pop()  # Backtrack to explore other combinations
```

## Approach 1: Iterative with Stack (Less Common)

### Solution
python
```python
# Time Complexity: O(2^n), where n is the number of candidates
# Space Complexity: O(k), where k is the average length of a combination
from typing import List

class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        candidates.sort()  # Sort the candidates to handle duplicates

        stack = [(0, target, -1)]  # Initial state: [current_sum, remaining_target, previous_index]
        current = []

        while stack:
            current_sum, remaining_target, prev_index = stack.pop()

            if remaining_target == 0:
                result.append(list(current))  # Add valid combinations
                continue

            # Remaining Iterative Building logic.
```

(Note: The iterative solution implementation is incomplete in the provided Java code and hence remains so in Python.)

