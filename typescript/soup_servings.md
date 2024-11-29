# 808. [Soup Servings](https://leetcode.com/problems/soup-servings/)

## Approach 1: Recursive with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
function soupServings(N: number): number {
    const memo = new Map<string, number>();

    if (N >= 5000) return 1.0; // Threshold optimization

    function serve(a: number, b: number): number {
        if (a <= 0 && b <= 0) return 0.5; // Both soups are finished
        if (a <= 0) return 1; // Only soup A finishes
        if (b <= 0) return 0; // Only soup B finishes

        const key = `${a},${b}`;
        if (memo.has(key)) return memo.get(key)!;

        const probability = 0.25 * (
            serve(a - 100, b) +
            serve(a - 75, b - 25) +
            serve(a - 50, b - 50) +
            serve(a - 25, b - 75)
        );

        memo.set(key, probability);
        return probability;
    }

    return serve(N, N);
}
```

## Approach 2: Iterative with Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
function soupServings(N: number): number {
    if (N >= 5000) return 1.0; // Threshold optimization

    N = Math.ceil(N / 25); // Reduce and round up

    const dp: number[][] = Array.from({ length: N + 1 }, () => Array(N + 1).fill(0));
    dp[0][0] = 0.5; // Base case: both soups are empty
    for (let i = 1; i <= N; i++) {
        dp[i][0] = 0; // Soup B empty first
        dp[0][i] = 1; // Soup A empty first
    }

    for (let i = 1; i <= N; i++) {
        for (let j = 1; j <= N; j++) {
            dp[i][j] = 0.25 * (
                getProbability(dp, i - 4, j) + 
                getProbability(dp, i - 3, j - 1) + 
                getProbability(dp, i - 2, j - 2) + 
                getProbability(dp, i - 1, j - 3)
            );
        }
    }

    return dp[N][N];
}

function getProbability(dp: number[][], a: number, b: number): number {
    if (a <= 0 && b <= 0) return 0.5; // Both finish at the same time
    if (a <= 0) return 1; // Only soup A finishes
    if (b <= 0) return 0; // Only soup B finishes
    return dp[a][b];
}
```

