
[Leetcode Problem 494: Target Sum](https://leetcode.com/problems/target-sum/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Memoization (Top-Down DP) Approach](#memoization-top-down-dp-approach)
3. [Dynamic Programming (Bottom-Up DP) Approach](#dynamic-programming-bottom-up-dp-approach)

### Recursive Approach

#### Intuition:
The idea is to explore all possible ways to assign + or - before each number in the array to see if we can achieve the target sum. We use a recursive function that at each step considers the possibility of adding or subtracting the current number.

#### Steps:
- Define a recursive function that takes the current index and the accumulated sum as arguments.
- Base case: If the current index is equal to the length of the array, check if the accumulated sum equals the target sum.
- Recursive case: Consider the result of adding the current number to the sum and subtracting it.
- Return the total number of ways from both recursive results.

```go
func findTargetSumWays(nums []int, target int) int {
    return calculate(nums, 0, 0, target)
}

func calculate(nums []int, i, sum, target int) int {
    // Base case: if we've gone through all numbers
    if i == len(nums) {
        if sum == target {
            return 1
        }
        return 0
    }
    // Recurse by adding and subtracting the number at this index
    return calculate(nums, i+1, sum+nums[i], target) + calculate(nums, i+1, sum-nums[i], target)
}
```

- **Time Complexity**: O(2^n), where n is the number of elements in nums.
- **Space Complexity**: O(n), where n is the depth of the recursion tree.

### Memoization (Top-Down DP) Approach

#### Intuition:
In the recursive approach, we might solve the same subproblem multiple times. We can optimize this by storing already-computed results for a given index and sum to avoid redundant calculations.

#### Steps:
- Use a map to store results of subproblems.
- Convert the recursive function to use the map to cache already computed results.

```go
func findTargetSumWays(nums []int, target int) int {
    memo := make(map[string]int)
    return calculateMemo(nums, 0, 0, target, memo)
}

func calculateMemo(nums []int, i, sum, target int, memo map[string]int) int {
    key := fmt.Sprintf("%d,%d", i, sum)
    if val, exists := memo[key]; exists {
        return val
    }
    if i == len(nums) {
        if sum == target {
            return 1
        }
        return 0
    }
    // storing the calculated value in the memo map
    result := calculateMemo(nums, i+1, sum+nums[i], target, memo) + calculateMemo(nums, i+1, sum-nums[i], target, memo)
    memo[key] = result
    return result
}
```

- **Time Complexity**: O(n * s), where n is the number of elements in nums and s is the range of possible sums.
- **Space Complexity**: O(n * s), for the memoization table.

### Dynamic Programming (Bottom-Up DP) Approach

#### Intuition:
Transform the problem into a subset sum problem. Partition the array into two subsets with sums `P` and `N`, such that `P - N = target`. This leads to a transformed equation: `P = (total_sum + target) / 2`.

#### Steps:
- Check if the target can be achieved based on the conditions derived from transformations.
- Use a dp array where dp[j] tracks the number of ways sum j can be achieved with the available array numbers.
- Iterate through the numbers updating the dp array accordingly.

```go
func findTargetSumWays(nums []int, target int) int {
    totalSum := 0
    for _, num := range nums {
        totalSum += num
    }
    // Check if it's impossible to partition into the desired subsets
    if (totalSum+target)%2 == 1 || totalSum < target {
        return 0
    }
    
    sum := (totalSum + target) / 2
    dp := make([]int, sum+1)
    dp[0] = 1 // There's one way to make the sum zero: using no numbers

    for _, num := range nums {
        for j := sum; j >= num; j-- {
            dp[j] += dp[j-num]
        }
    }
    return dp[sum]
}
```

- **Time Complexity**: O(n * s), where n is the number of elements, and s is the subset sum derived.
- **Space Complexity**: O(s), where s is the target subset sum.

Each solution improves upon the last by reducing the number of unnecessary calculations and optimizing the space used, ensuring better performance with increasing input sizes.

