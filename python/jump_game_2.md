# 45. [Jump Game II](https://leetcode.com/problems/jump-game-ii/)

## Approach: Greedy Algorithm

### Solution
python
```python
# Time Complexity: O(n), where n is the length of the array
# Space Complexity: O(1)
class Solution:
    def jump(self, nums: List[int]) -> int:
        jumps = 0  # Number of jumps required
        current_end = 0  # Farthest point that can be reached with current number of jumps
        farthest = 0  # Farthest point that can be reached from the current position

        for i in range(len(nums) - 1):
            farthest = max(farthest, i + nums[i])  # Update the farthest point reachable

            # If we reach the end of the current range
            if i == current_end:
                jumps += 1  # Increment the jump count
                current_end = farthest  # Update the current range to the farthest point

        return jumps  # Return the total number of jumps
```

