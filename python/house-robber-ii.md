# [Leetcode Problem 213: House Robber II](https://leetcode.com/problems/house-robber-ii/)

## Approaches
- [Approach 1: Dynamic Programming with Two Passes](#approach-1-dynamic-programming-with-two-passes)
- [Approach 2: Optimized Dynamic Programming with Constant Space](#approach-2-optimized-dynamic-programming-with-constant-space)

### Approach 1: Dynamic Programming with Two Passes

**Intuition:**

This problem is a variation of the House Robber I problem, but with the houses arranged in a circle. This means that the first and last houses cannot be robbed together, which adds a constraint. The idea is to break the problem into two linear House Robber problems and solve each using dynamic programming.

1. Consider the array excluding the first house.
2. Consider the array excluding the last house.
3. Solve the House Robber problem for both arrays and take the maximum of the two solutions.

#### Steps:

- Create a helper function to solve the House Robber I problem.
- Call this helper function twice:
  - Once for the array excluding the first element.
  - Once for the array excluding the last element.
- Return the maximum value obtained from either configuration.

```python
def rob(nums):
    # Base case, if there are no houses, maximum amount to rob is 0.
    if not nums:
        return 0
    
    # If there's only one house, the max value to rob is just that house's value.
    if len(nums) == 1:
        return nums[0]
    
    def rob_linear(houses):
        prev1, prev2 = 0, 0
        for current_house_value in houses:
            # Calculate max money that can be robbed if including this house
            new_house = max(prev2 + current_house_value, prev1)
            prev2 = prev1
            prev1 = new_house
        return prev1
    
    # Rob houses from 0 to n-1 and houses from 1 to n to account for the circle.
    result1 = rob_linear(nums[:-1])  # Rob all but last house
    result2 = rob_linear(nums[1:])   # Rob all but first house
    return max(result1, result2)

# Time Complexity: O(n), where n is the number of houses, since we solve two linear House Robber problems.
# Space Complexity: O(1), since only a constant amount of space is used.
```

### Approach 2: Optimized Dynamic Programming with Constant Space

**Intuition:**

Following the same principle as Approach 1, but recognizing the pattern: Only the last two calculations are necessary at any one time, so we can skip maintaining a full dynamic programming table and use two variables to hold intermediate states for the two subproblems.

#### Steps:

- Similar to before, create a helper for linear House Robber using two variables to keep track of previous maximums.
- Analyze two segments of the array as before.
- Use the helper to find max rob values for both segments and return the larger one.

- This solution is space efficient, as it doesn't use extra arrays to store calculations.

```python
def rob(nums):
    # Base case, if there are no houses, maximum amount to rob is 0.
    if not nums:
        return 0

    # If there's only one house, maximum amount to rob is just that house's value.
    if len(nums) == 1:
        return nums[0]

    def rob_linear(houses):
        # Initialize variables to store the maximum amount robbed so far.
        prev1, prev2 = 0, 0
        for current_house_value in houses:
            # Calculate max money that can be robbed if including this house.
            new_house = max(prev2 + current_house_value, prev1)
            # Move the values forward for next iteration.
            prev2 = prev1
            prev1 = new_house
        return prev1

    # Rob houses from 0 to n-1 and houses from 1 to n to account for the circle.
    result1 = rob_linear(nums[:-1])  # Rob all but last house
    result2 = rob_linear(nums[1:])   # Rob all but first house
    return max(result1, result2)

# Time Complexity: O(n), where n is the number of houses.
# Space Complexity: O(1), since only two variables are used for calculation.
```

Both of these approaches effectively handle the circular constraint of the problem by dividing it into smaller non-circular problems and then taking the optimal result from those subproblems.

