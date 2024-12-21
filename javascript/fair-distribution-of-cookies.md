# [Leetcode 2305: Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

## Approaches
- [Approach 1: Backtracking](#approach-1-backtracking)
- [Approach 2: Dynamic Programming with Bitmasking](#approach-2-dynamic-programming-with-bitmasking)

---

## Approach 1: Backtracking

### Intuition

The problem at hand is to distribute a set of cookies among k children such that the unfairness, defined as the maximum sum of cookies assigned to a single child, is minimized. Straightforward greedy approaches won't work here since the aim is to minimize the maximum sum across all distributions, not just optimize based on immediate choices. Thus, a backtracking approach can explore all possible distributions, tracking the optimal configuration.

### Algorithm Steps

1. **Base Case**: Use recursion to backtrack all possible distributions of cookies to k children.
2. **Recursive Distribution**: For each cookie, iterate over each child and:
   - Assign the cookie to the child.
   - Recurse with the next cookie.
   - Backtrack by removing the cookie from the child.
3. **Track Minimum Unfair Distribution**: Keep a running minimum of the maximum cookies assigned to any single child configuration.
4. **Prune**: If at any point the maximum assigned cookies to a child equals or exceeds the current minimum unfairness, discard that path (prune it). 

### Code

```javascript
function distributeCookies(cookies, k) {
    let result = Number.MAX_SAFE_INTEGER;

    function backtrack(index, distribution) {
        // When all cookies are distributed
        if (index === cookies.length) {
            // Determine the unfairness of this distribution
            let maxCookies = Math.max(...distribution);
            // Update the overall result (minimize the maximum cookies with any child)
            result = Math.min(result, maxCookies);
            return;
        }

        for (let i = 0; i < k; i++) {
            // Distribute current cookie to the i-th child
            distribution[i] += cookies[index];
            // Recurse to distribute next cookies
            backtrack(index + 1, distribution);
            // Remove the cookie from this child (backtrack)
            distribution[i] -= cookies[index];
            // Optimization: if this child has no cookies initially, skip further as the distributions will not be minimal
            if (distribution[i] === 0) break;
        }
    }

    backtrack(0, new Array(k).fill(0));
    return result;
}
```

### Time Complexity

- **Time**: \(O(k^n)\), where n is the number of cookies. Since we recursively distribute cookies to k children, this might result in a lengthy but exhaustive search.
- **Space**: \(O(n + k)\), due to the recursive stack and numerical array to track distribution.

---

## Approach 2: Dynamic Programming with Bitmasking

### Intuition

While backtracking provides a powerful approach to ensuring all possible distributions are considered, dynamic programming (DP) can provide a more efficient search strategy by leveraging previously calculated results, reducing duplication of work. Bitmasks represent distribution states to save memory and computation.

### Algorithm Steps

1. **State Representation**: Use a bitmask to track which cookies have been distributed.
2. **Define State Function**: For each DP state, calculate minimum unfairness by considering each possible new cookie allocation based on bitmasks.
3. **Transition**: For each state, calculate potential new state unfairness and decide on updating the state only if it offers an improvement.

### Code

```javascript
function distributeCookies(cookies, k) {
    const n = cookies.length;
    const sum = (1 << n);
    const dp = Array.from({ length: sum }, () => new Array(k).fill(Number.MAX_SAFE_INTEGER));

    const total = Array(sum).fill(0);

    // Pre-calculate total cookies count for each subset (bitmask)
    for (let mask = 0; mask < sum; mask++) {
        total[mask] = 0;
        for (let i = 0; i < n; i++) {
            if ((mask & (1 << i)) !== 0) {
                total[mask] += cookies[i];
            }
        }
    }

    // Base case: no cookies, no unfairness
    dp[0][0] = 0;

    for (let mask = 0; mask < sum; mask++) {
        for (let j = 0; j < k; j++) {
            // Skip invalid state
            if (dp[mask][j] === Number.MAX_SAFE_INTEGER) continue;
            
            // Consider next cookie set
            for (let subset = mask; subset < sum; subset = (subset + 1) | mask) {
                const currentMax = Math.max(dp[mask][j], total[subset ^ mask]);
                dp[subset][j + 1] = Math.min(dp[subset][j + 1], currentMax);
            }
        }
    }

    return dp[sum - 1][k - 1];
}
```

### Time Complexity

- **Time**: \(O(n \times 2^n \times k)\). Given n cookies, each cookie can either be included or not (hence, \(2^n\) possible states), evaluated for k children distributions.
- **Space**: \(O(k \times 2^n)\) for maintaining DP states.

Both approaches demonstrate different trade-offs. Backtracking provides exhaustive solutions but with potential inefficiencies for larger datasets. On the other hand, DP with bitmasking offers improved computation time but requires more complex implementation and memory usage. Choose based on problem constraints and desired trade-offs.

