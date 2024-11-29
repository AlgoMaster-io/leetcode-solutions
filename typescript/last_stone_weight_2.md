# 1049. [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Approach 1: Dynamic Programming with a Set

### Solution
typescript
```typescript
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
class Solution {
    public lastStoneWeightII(stones: number[]): number {
        let dp = new Set<number>();
        dp.add(0);
        let sum = 0;

        // Iterate over each stone
        for (let stone of stones) {
            const newDp = new Set<number>();
            // Update possible sums with and without the current stone
            for (let s of dp) {
                newDp.add(s + stone);
                newDp.add(s - stone);
            }
            // Move to the new set of possible sums
            dp = newDp;
            sum += stone;
        }

        // Find the minimum possible non-negative sum (half the total sum)
        let minAbs = Number.MAX_SAFE_INTEGER;
        for (let s of dp) {
            if (s >= 0) {
                minAbs = Math.min(minAbs, s);
            }
        }
        
        return minAbs;
    }
}
```

## Approach 2: Dynamic Programming with Boolean Array

### Solution
typescript
```typescript
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
class Solution {
    public lastStoneWeightII(stones: number[]): number {
        let sum = 0;
        for (let stone of stones) {
            sum += stone;
        }
        const dp = new Array<boolean>(Math.floor(sum / 2) + 1).fill(false);
        dp[0] = true;

        // Evaluate possible subset sums
        for (let stone of stones) {
            for (let j = dp.length - 1; j >= stone; j--) {
                dp[j] = dp[j] || dp[j - stone];
            }
        }

        // Find the largest possible subset sum that is <= sum/2
        for (let j = dp.length - 1; j >= 0; j--) {
            if (dp[j]) {
                return sum - 2 * j;
            }
        }

        return 0;
    }
}
```

## Approach 3: Optimized DP with Single Array

### Solution
typescript
```typescript
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
class Solution {
    public lastStoneWeightII(stones: number[]): number {
        let sum = 0;
        for (let stone of stones) {
            sum += stone;
        }
        const target = Math.floor(sum / 2);
        const dp = new Array<number>(target + 1).fill(0);

        // Calculate optimized subset sums
        for (let stone of stones) {
            for (let j = target; j >= stone; j--) {
                dp[j] = Math.max(dp[j], dp[j - stone] + stone);
            }
        }

        // Result is the difference between total sum and twice the best subset sum
        return sum - 2 * dp[target];
    }
}
```

