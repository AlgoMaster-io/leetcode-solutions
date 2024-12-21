# [Leetcode Problem 45: Jump Game II](https://leetcode.com/problems/jump-game-ii/)

## Table of Contents
- [Approach 1: Greedy Algorithm](#approach-1-greedy-algorithm)
- [Approach 2: Optimized Greedy Algorithm (Optimal)](#approach-2-optimized-greedy-algorithm-optimal)

## Approach 1: Greedy Algorithm

### Intuition
The problem asks for the minimum number of jumps needed to reach the last index. A straightforward greedy approach is to always make a jump to the farthest possible index that can be reached, and continue this process until the end of the array is reached.

### Steps
1. Initialize a variable `jumps` to count the number of jumps.
2. Use two pointers:
   - `current_end` to mark the end of the range that can be reached with the current number of jumps.
   - `farthest` to track the farthest point that can be reached with the next jump.
3. Iterate through the array till the second last index because from the last index, no jump is needed.
4. Update the `farthest` pointer with the maximum of its current value and `i + nums[i]`.
5. If you reach the `current_end`, it means a jump needs to be made. Increment `jumps` and update `current_end` to `farthest`.

### Code
```cpp
#include <vector>
#include <algorithm>
using namespace std;

int jump(vector<int>& nums) {
    int jumps = 0;         // To count the minimum number of jumps
    int current_end = 0;   // Current end of the jump
    int farthest = 0;      // Farthest point that can be reached

    for (int i = 0; i < nums.size() - 1; ++i) {
        // Update farthest point
        farthest = max(farthest, i + nums[i]);

        // Check if we need to jump
        if (i == current_end) {
            jumps++;           // Increment the jump counter
            current_end = farthest; // Update the end of the jump range
            
            // If the farthest point reached is beyond or at the last index, break
            if (current_end >= nums.size() - 1) break;
        }
    }

    return jumps;
}
```

### Time Complexity
- **Time Complexity**: O(n), where n is the number of elements in the array. We pass through the array just once.

### Space Complexity
- **Space Complexity**: O(1), no extra space is used.

## Approach 2: Optimized Greedy Algorithm (Optimal)

### Intuition
The above greedy solution is already optimal. Here we make decisions at each step based on the maximum distance that can be achieved with the next jump. It only requires a single pass through the array, making it the most efficient approach given the constraints.

### Additional Details
- The approach doesn't change but detailed understanding is provided on why it works optimally. 
- We essentially partition the problem into reaching further sections of the array with the least number of jumps.

### Code
The code stays the same as the first greedy approach.

### Time Complexity
- **Time Complexity**: O(n)
  - Only a single pass through the array is required.

### Space Complexity
- **Space Complexity**: O(1)

This concludes the available approaches for this problem, providing both the intuition and rationale behind their efficiency. The greedy algorithm proposed is optimal and ensures minimal jumps to reach the end of the array.

