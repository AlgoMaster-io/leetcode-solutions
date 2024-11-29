# 45. [Jump Game II](https://leetcode.com/problems/jump-game-ii/)

## Approach: Greedy Algorithm

### Solution
typescript
```typescript
// Time Complexity: O(n), where n is the length of the array
// Space Complexity: O(1)
class Solution {
    jump(nums: number[]): number {
        let jumps = 0; // Number of jumps required
        let currentEnd = 0; // Farthest point that can be reached with current number of jumps
        let farthest = 0; // Farthest point that can be reached from the current position

        for (let i = 0; i < nums.length - 1; i++) {
            farthest = Math.max(farthest, i + nums[i]); // Update the farthest point reachable

            // If we reach the end of the current range
            if (i === currentEnd) {
                jumps++; // Increment the jump count
                currentEnd = farthest; // Update the current range to the farthest point
            }
        }

        return jumps; // Return the total number of jumps
    }
}
```

