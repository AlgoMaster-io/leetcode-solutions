# 45. [Jump Game II](https://leetcode.com/problems/jump-game-ii/)

## Approach: Greedy Algorithm

### Solution
```cpp
// Time Complexity: O(n), where n is the length of the array
// Space Complexity: O(1)
class Solution {
public:
    int jump(vector<int>& nums) {
        int jumps = 0; // Number of jumps required
        int currentEnd = 0; // Farthest point that can be reached with current number of jumps
        int farthest = 0; // Farthest point that can be reached from the current position

        for (int i = 0; i < nums.size() - 1; i++) {
            farthest = max(farthest, i + nums[i]); // Update the farthest point reachable

            // If we reach the end of the current range
            if (i == currentEnd) {
                jumps++; // Increment the jump count
                currentEnd = farthest; // Update the current range to the farthest point
            }
        }

        return jumps; // Return the total number of jumps
    }
};
```

