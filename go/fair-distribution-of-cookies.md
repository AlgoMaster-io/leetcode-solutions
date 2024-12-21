# [Leetcode 2305: Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Backtracking (Optimized)](#approach-2-backtracking-optimized)

### Approach 1: Brute Force

This naive solution tries all possible distributions and then determines the minimum unfairness. It evaluates every possible way to distribute cookies to the children.

#### Intuition
1. We can distribute cookies in any order, and we aim to minimize the maximum number of cookies any child has.
2. We will try every possible distribution one by one and update our answer with the minimum maximum cookies distributed.

#### Steps
- Generate all possible distributions of cookies using a recursive approach.
- For each distribution, calculate unfairness, defined as the maximum cookies any child has.
- Track the minimum unfairness across all distributions.

#### Complexity
- **Time Complexity:** O(k^n), where k is the number of children and n is the number of cookies. This is because each cookie can go to any of the k children.
- **Space Complexity:** O(k), for storing cookies assigned to each child.

```go
func distributeCookies(cookies []int, k int) int {
    // Function to calculate distribution using the backtrack algorithm
    var backtrack func(int, []int) int
    backtrack = func(index int, distribution []int) int {
        // If all cookies have been considered, return the maximum cookies a child has
        if index == len(cookies) {
            maxCookies := 0
            for _, dist := range distribution {
                maxCookies = max(maxCookies, dist)
            }
            return maxCookies
        }

        // Start with an initially large unfairness
        unfairness := int(^uint(0) >> 1) // max int

        // Try assigning the current cookie to each child
        for i := 0; i < k; i++ {
            // Assign cookie to child i and proceed to the next cookie
            distribution[i] += cookies[index]
            unfairness = min(unfairness, backtrack(index+1, distribution))
            // Backtrack: unassign cookie to child i
            distribution[i] -= cookies[index]
        }
        return unfairness
    }

    distribution := make([]int, k)
    return backtrack(0, distribution)
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Approach 2: Backtracking (Optimized)

Refine the brute-force method by realizing that in certain configurations, further searches can be pruned a lot sooner.

#### Intuition
1. Similar to brute force, but we avoid distributing the current cookie to an already overloaded child.
2. With a little logical enhancement, our recursive function can prune unnecessary states where we see no possible better outcome.

#### Steps
- Use a recursive backtracking approach but skip unnecessary configurations.
- Prune recursive paths where the current distribution is already worse than the best found so far.

#### Complexity
- **Time Complexity:** O((k!) * n). With optimal pruning, could be reduced further in practical scenarios.
- **Space Complexity:** O(k), similar to brute force.

```go
func distributeCookiesOptimized(cookies []int, k int) int {
    // To store the best (minimum) fairness found
    answer := int(^uint(0) >> 1)

    // Sort the cookies for better pruning in the backtracking step
    sort.Sort(sort.Reverse(sort.IntSlice(cookies)))

    // Function for backtracking with pruning
    var backtrack func(int, []int, int)
    backtrack = func(index int, distribution []int, maxCookies int) {
        // If all cookies are distributed, try to update the best possible answer
        if index == len(cookies) {
            answer = min(answer, maxCookies)
            return
        }

        // Try assigning cookie to each child
        for i := 0; i < k; i++ {
            // Check if assigning is beneficial
            if distribution[i]+cookies[index] >= answer {
                continue
            }
            distribution[i] += cookies[index]
            // Proceed to distribute next cookie
            backtrack(index+1, distribution, max(distribution[i], maxCookies))
            // Backtrack: Remove that cookie from the child
            distribution[i] -= cookies[index]
            // Early termination: If the child was initially allocated nothing, break
            if distribution[i] == 0 {
                break
            }
        }
    }

    distribution := make([]int, k)
    backtrack(0, distribution, 0)
    return answer
}
```

The above solution optimally backtracks by intelligently pruning paths that will not lead to a better result than already achieved.

