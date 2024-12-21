# [Leetcode 45: Jump Game II](https://leetcode.com/problems/jump-game-ii/)

## Navigation
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Greedy Algorithm](#approach-2-greedy-algorithm)

---

## Approach 1: Brute Force

### Intuition
In the brute force approach, we try every possible jump from the current index to see if we can reach the end of the array in the minimum number of hops. Starting from each index, simulate every jump possible (up to the value at that index), and recursively calculate the number of jumps needed. This approach explores all possible combinations and selects the one with the minimum jumps.

### Code
```csharp
public class Solution {
    public int Jump(int[] nums) {
        return JumpFromPosition(0, nums);
    }
    
    private int JumpFromPosition(int position, int[] nums) {
        // Base case: If we have reached the last index, no more jumps are needed.
        if (position >= nums.Length - 1) {
            return 0;
        }
        
        int maxJump = nums[position];
        int minJumps = Int32.MaxValue;
        
        // Try every jump from current position
        for (int i = 1; i <= maxJump; i++) {
            int jumps = JumpFromPosition(position + i, nums);
            // Find the minimum number of jumps needed
            if (jumps < minJumps) {
                minJumps = jumps + 1; // +1 for the current jump
            }
        }
        
        return minJumps;
    }
}
```

### Time and Space Complexities
- **Time Complexity:** \(O(n^n)\), where \(n\) is the length of the array. This is because from each index, we are exploring all possible jumps, making it highly inefficient.
- **Space Complexity:** \(O(n)\), the depth of the recursion stack can go up to \(n\).

---

## Approach 2: Greedy Algorithm

### Intuition
The problem can be optimized using a greedy approach. The idea is to keep track of the farthest point that can be reached from the current step. We jump to the farthest reachable point seen so far. We only increment the number of jumps when our current jump ends (i.e., when we reach the end of the current furthest reach).

### Code
```csharp
public class Solution {
    public int Jump(int[] nums) {
        int jumps = 0; // Number of jumps made so far
        int currentEnd = 0; // Farthest we can reach in the current jump
        int farthest = 0; // Farthest we can reach at all
        
        for (int i = 0; i < nums.Length - 1; i++) {
            // Continuously find how far we can reach from the current point
            farthest = Math.Max(farthest, i + nums[i]);
            
            // If we have reached the end of the current jump,
            // we must do a jump, so we update the current end
            if (i == currentEnd) {
                jumps++;
                currentEnd = farthest;
                
                // Early exit if we've already reached the end
                if (currentEnd >= nums.Length - 1) {
                    break;
                }
            }
        }
        
        return jumps;
    }
}
```

### Time and Space Complexities
- **Time Complexity:** \(O(n)\), where \(n\) is the length of the array, because we go through the array once.
- **Space Complexity:** \(O(1)\), as we are using a constant amount of extra space.

---

By using the Greedy Algorithm approach, we significantly improve the efficiency of solving the problem, achieving linear time complexity. This is the optimal approach for this problem.

