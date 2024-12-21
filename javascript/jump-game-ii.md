## [Jump Game II - Leetcode Problem 45](https://leetcode.com/problems/jump-game-ii/)

### Navigation:
1. [Approach 1: Brute Force Recursion](#approach-1)
2. [Approach 2: Dynamic Programming](#approach-2)
3. [Approach 3: Greedy Approach](#approach-3)

---

### Approach 1: Brute Force Recursion

**Intuition:**

The most basic way to solve this problem is through recursion, exploring every possible jump. From each position, we try all jump lengths and recursively find the minimum number of jumps needed to reach the last index.

- The base case is when we reach or pass the last index (return 0 jumps needed as we are already at the end).
- For each index, explore all reachable indices and calculate the number of jumps required from each.

**Code:**

```javascript
function jump(nums) {
    const n = nums.length;
    
    function minJumps(position) {
        if (position >= n - 1) {
            return 0; // No jump needed if we are at or beyond the last position.
        }

        let minSteps = Infinity;
        const maxJump = nums[position]; // Maximum jump length from current position.
        for (let nextPosition = position + 1; nextPosition <= position + maxJump; nextPosition++) {
            minSteps = Math.min(minSteps, 1 + minJumps(nextPosition));
        }

        return minSteps;
    }

    return minJumps(0);
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n^n) - Every index can initiate jumps to other indices causing exponential number of calls.
- **Space Complexity:** O(n) - Stack space due to recursion.

---

### Approach 2: Dynamic Programming

**Intuition:**

To optimize the recursive approach, we can use dynamic programming with memoization here. By storing results of already computed states, we make sure we do not calculate the same state more than once.

- Create a memoization array where `memo[i]` stores the minimum number of jumps needed from index `i` to the end.
- Start filling from the end to reduce overlapping subproblems.

**Code:**

```javascript
function jump(nums) {
    const n = nums.length;
    const memo = new Array(n).fill(n); // Initialize with max possible jumps which is n.

    memo[n - 1] = 0; // No jump needed if we're already at the end.

    for (let i = n - 2; i >= 0; i--) {
        const maxJump = Math.min(n - 1, i + nums[i]);
        for (let j = i + 1; j <= maxJump; j++) {
            memo[i] = Math.min(memo[i], 1 + memo[j]);
        }
    }
    
    return memo[0];
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n^2) - In the worst case, double nested for loops iterate over all elements.
- **Space Complexity:** O(n) - For the memoization array.

---

### Approach 3: Greedy Approach

**Intuition:**

A more optimal solution leverages the greedy algorithm. The essence of the greedy approach is to make the jump that covers the longest distance at each step.

- Track the farthest index that can be reached at the current step.
- Increase the jump count whenever you must move beyond the current farthest reach.

**Code:**

```javascript
function jump(nums) {
    const n = nums.length;
    if (n < 2) return 0; // No jumps needed if we are already at the end.
    
    let jumps = 0;
    let currentEnd = 0;
    let farthest = 0;

    for (let i = 0; i < n - 1; i++) {
        farthest = Math.max(farthest, i + nums[i]); // Update the farthest we can reach from position i.

        if (i === currentEnd) {
            jumps++; // We need to make a jump since we've reached the end of the current ability.
            currentEnd = farthest; // Set the new range up to the farthest we can now reach.
        }
    }
    
    return jumps;
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n) - Single pass through the array.
- **Space Complexity:** O(1) - Only a few variables used for tracking indices and counts.

---

These solutions range from an exhaustive search to an optimal greedy strategy for minimizing jumps in the given array. Understanding each will give clear insights into different problem-solving approaches for the Jump Game II problem.

