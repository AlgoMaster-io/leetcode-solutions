# [Leetcode 2305: Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

## Approaches:

- [Brute Force](#brute-force)
- [Backtracking](#backtracking)

### Brute Force

#### Intuition:
The brute force approach involves generating all possible ways to distribute the given bags of cookies to `k` children and finding the distribution where the cookie bags are most evenly distributed based on the maximum number of cookies one child has received. Since we can distribute each cookie bag to any child, this results in a combination problem where we are essentially dividing `n` cookie bags among `k` children in every possible manner.

#### Steps:
1. Enumerate all possible distributions of cookie bags among the children by assigning each bag to any child.
2. For each distribution, calculate the maximum number of cookies any single child receives.
3. Track the minimum among these maxima, which represents the best (fairest) distribution.

#### Code:
```typescript
function distributeCookies(cookies: number[], k: number): number {
  const childrensCookies = new Array(k).fill(0);
  const totalBags = cookies.length;

  function distribute(index: number): number {
    if (index === totalBags) {
      // Return the maximum cookies received by any child in this configuration
      return Math.max(...childrensCookies);
    }

    let result = Number.MAX_VALUE;
    // Assign the current cookie bag to each child and check the minimum largest distribution
    for (let i = 0; i < k; i++) {
      childrensCookies[i] += cookies[index];
      const currentMax = distribute(index + 1);
      result = Math.min(result, currentMax);
      // Backtracking step
      childrensCookies[i] -= cookies[index];
    }

    return result;
  }

  return distribute(0);
}
```

#### Time Complexity:
- The brute force approach is exponential with respect to the number of cookie bags `n` and `k` children, i.e., `O(k^n)`.

#### Space Complexity:
- The space complexity is `O(k)` due to the storage of the cookies distribution among the children.

### Backtracking

#### Intuition:
This approach improves upon the brute force method by reducing unnecessary calculations through pruning. By ensuring early in the recursion that the current distribution is unlikely to yield an optimal solution, we avoid exploring unpromising paths. We implement this by stopping allocation if the current maximum is already higher than a previously found solution.

#### Steps:
1. Utilise a recursive function to attempt distributing each cookie bag starting with the least filled children first, aiming for minimizing the maximum in each recursive call.
2. Prune the recursion if a child has more cookies than the current best known result.
3. Return the minimum of all possible distributions.

#### Code:
```typescript
function distributeCookies(cookies: number[], k: number): number {
  const childrensCookies = new Array(k).fill(0);
  let minUnfairness = Number.MAX_VALUE; // Track the minimum unfairness 

  function distribute(index: number): void {
    if (index === cookies.length) {
      const currentUnfairness = Math.max(...childrensCookies);
      minUnfairness = Math.min(minUnfairness, currentUnfairness);
      return;
    }

    for (let i = 0; i < k; i++) {
      if (childrensCookies[i] + cookies[index] >= minUnfairness) continue; // Prune this path if it can't improve the result
      childrensCookies[i] += cookies[index];
      distribute(index + 1);
      childrensCookies[i] -= cookies[index]; // Backtracking step
    }
  }

  distribute(0);
  return minUnfairness;
}
```

#### Time Complexity:
- This approach still remains exponential in the worst case, i.e., `O(k^n)`, but in practice, pruning can significantly reduce the search space.

#### Space Complexity:
- The space complexity is `O(k)` to store the current cookie distribution among the children.

