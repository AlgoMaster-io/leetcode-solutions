## [Leetcode 808: Soup Servings](https://leetcode.com/problems/soup-servings/)

### Approaches
- [Approach 1: Recursive DFS with Memoization](#approach-1-recursive-dfs-with-memoization)

---

### Approach 1: Recursive DFS with Memoization

#### Intuition
The goal is to determine the probability that Soup A will run out before Soup B. Given the serving sizes, a brute-force approach might consider every possible serving combination, but this would be highly inefficient. Instead, a recursive approach with memoization can significantly reduce the computation time by caching already computed results. 

The problem can be recursively defined where for each serving step, we explore all possible serving combinations. The base cases where either soup A or B runs out can be directly returned, and results for any particular state can be cached to avoid redundant calculations.

#### Solution

```typescript
function soupServings(N: number): number {
  // If N is too large, the probability approaches a constant
  if (N > 5000) return 1.0;

  // Define a memoization cache to store computed probabilities
  const memo: Map<string, number> = new Map();
  
  // Normalizing serving size to avoid unnecessary recursive depth
  const normalize = (n: number) => Math.ceil(n / 25);

  // Recursive DFS function with memoization
  function dfs(a: number, b: number): number {
    // Create a unique key for the current state
    const key = `${a},${b}`;
    // Check if result is already computed
    if (memo.has(key)) return memo.get(key)!;

    // Base cases:
    if (a <= 0 && b <= 0) return 0.5; // Both run out simultaneously
    if (a <= 0) return 1.0;           // A runs out first
    if (b <= 0) return 0.0;           // B runs out first
    
    // Recursively compute probability for each serving size combination
    const prob = 0.25 * (
      dfs(a - 4, b) +
      dfs(a - 3, b - 1) +
      dfs(a - 2, b - 2) +
      dfs(a - 1, b - 3)
    );

    // Store the computed result in the cache
    memo.set(key, prob);
    return prob;
  }

  // Normalize the soup amounts to reduce problem size
  const n = normalize(N);
  return dfs(n, n);
}
```

#### Time and Space Complexity
- **Time Complexity:** O(n^2), where n = N/25. This accounts for the number of distinct states we might explore for each subproblem.
- **Space Complexity:** O(n^2) due to the memoization map storing results for each unique state (a, b).

In this approach, we utilize memoization to efficiently resolve redundancies in recursive calls and ensure each unique state is computed only once. As mentioned, due to the problem behavior, if `N` is extremely large, the probability tends towards 1, which is an important optimization to handle large inputs quickly.

