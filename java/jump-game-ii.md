[Leetcode Problem 45: Jump Game II](https://leetcode.com/problems/jump-game-ii/)

## Approaches

1. [Greedy Approach](#greedy-approach)
2. [Optimized Greedy Approach](#optimized-greedy-approach)

---

### Greedy Approach

**Intuition:**

The first solution is a simple greedy approach. The idea is to make the best move we can at each index. We traverse the array and at each step, we jump to the farthest position possible within the range of our current jump. By making the best move at each step (jumping the farthest possible), we ensure that we reach the end in the minimum number of jumps.

**Approach:**
- Start from the first position of the array and attempt to reach the last position with the minimum number of jumps.
- Use variables:
  - `jumps` to count the number of jumps.
  - `curEnd` to track the farthest index that can be reached with the current number of jumps.
  - `curFarthest` to track the farthest index that we could reach with an additional jump within the range of `curEnd`.
- Whenever we move beyond `curEnd`, it means we need to make a jump, so update `jumps` and `curEnd`.

```java
public class Solution {
    public int jump(int[] nums) {
        // Number of jumps needed to reach the end
        int jumps = 0;
        // Farthest index that can be reached with current number of jumps
        int curEnd = 0;
        // Farthest index that can be reached with an additional jump
        int curFarthest = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            // Update the farthest we can reach
            curFarthest = Math.max(curFarthest, i + nums[i]);
            // If we have reached the end of what we could jump with current `jumps`
            if (i == curEnd) {
                // Increment number of jumps
                jumps++;
                // Update the `curEnd` to the `farthest` we can reach with current number of jumps
                curEnd = curFarthest;
            }
        }
        return jumps;
    }
}
```

**Time Complexity:** O(n), since we are making a single pass through the array.

**Space Complexity:** O(1), as no extra space is used.

---

### Optimized Greedy Approach

**Intuition:**

In the optimized version of the greedy approach, we maintain only necessary variables to track the farthest reachable point and leverage this to calculate the minimum jumps on the go. By focusing on optimizing jump increments logically with conditions, this approach refines the previous solution for simplicity without affecting time complexity.

**Approach:**
- Similar to the first approach but is less explicitly managed with retained focus on optimal point reachability only.

```java
public class Solution {
    public int jump(int[] nums) {
        if (nums.length < 2) return 0;  // No jump needed if there's only one element.

        int jumps = 0, maxReach = 0, currentEnd = 0;

        for (int i = 0; i < nums.length - 1; i++) {
            // Always try to update the farthest we can reach
            maxReach = Math.max(maxReach, i + nums[i]);

            // If we have come to the end of our possible reach with current number of jumps
            if (i == currentEnd) {
                jumps++;  // We need an additional jump
                currentEnd = maxReach;  // Update current end to max reach
            }

            // Break if the maxReach exceeds or reaches the last node
            if (currentEnd >= nums.length - 1) {
                return jumps;
            }
        }
        
        return jumps;
    }
}
```

**Time Complexity:** O(n), iterating through the array once as before.

**Space Complexity:** O(1), no extra space apart from variables.

This completes our discussion on the given problem, highlighting the process improvements from a basic greedy approach to an optimized version while describing each step thoroughly.

