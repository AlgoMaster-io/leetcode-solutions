# [Leetcode 135: Candy](https://leetcode.com/problems/candy/)

## Approaches
- [Approach 1: Brute Force with Incremental Rewarding](#approach-1-brute-force-with-incremental-rewarding)
- [Approach 2: Two Pass Algorithm (Greedy Method)](#approach-2-two-pass-algorithm-greedy-method)

### Approach 1: Brute Force with Incremental Rewarding

**Intuition:**

The naive approach involves systematically distributing candies to ensure the conditions are met. Start by giving each child one candy, then iteratively adjust the distribution until all conditions are fulfilled:

1. Loop over the ratings multiple times.
2. Adjust candy distribution such that each child with a higher rating than their neighbors has more candies.
3. This process may require revisiting children multiple times and adjusting candies incrementally.

**Code:**

```typescript
function candy(ratings: number[]): number {
    const n = ratings.length;
    const candies = new Array(n).fill(1);

    let updated = true;
    
    // Continue until no updates are necessary
    while (updated) {
        updated = false;
        for (let i = 0; i < n; i++) {
            // Adjust for previous neighbor
            if (i > 0 && ratings[i] > ratings[i - 1] && candies[i] <= candies[i - 1]) {
                candies[i] = candies[i - 1] + 1;
                updated = true;
            }
            // Adjust for next neighbor
            if (i < n - 1 && ratings[i] > ratings[i + 1] && candies[i] <= candies[i + 1]) {
                candies[i] = candies[i + 1] + 1;
                updated = true;
            }
        }
    }

    return candies.reduce((a, b) => a + b, 0);
}
```

**Time Complexity:** O(n^2) - Each adjustment pass may iterate through the entire list, and the process could require multiple passes.  
**Space Complexity:** O(n) - Using additional space for the `candies` list.

### Approach 2: Two Pass Algorithm (Greedy Method)

**Intuition:**

This approach efficiently tackles the problem by leveraging two separate passes across the list. The first pass ensures that each child has more candies than the one to their left if they have a higher rating. The second pass moves from right to left, making sure that the condition is also satisfied for the right neighbors.

1. Create an array `candies`, initialized to 1 for each child.
2. First pass (left to right): Ensure that if a child has a higher rating than the one before, they receive more candies.
3. Second pass (right to left): Ensure that if a child has a higher rating than the one after, they receive more candies.

**Code:**

```typescript
function candy(ratings: number[]): number {
    const n = ratings.length;
    const candies = new Array(n).fill(1);

    // Left to right pass
    for (let i = 1; i < n; i++) {
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1;
        }
    }

    // Right to left pass
    for (let i = n - 2; i >= 0; i--) {
        if (ratings[i] > ratings[i + 1]) {
            candies[i] = Math.max(candies[i], candies[i + 1] + 1);
        }
    }

    return candies.reduce((a, b) => a + b, 0);
}
```

**Time Complexity:** O(n) - Each pass traverses the list once.  
**Space Complexity:** O(n) - The `candies` array requires additional space.

### Conclusion

The brute force method provides a straightforward solution but is inefficient for large inputs. The two-pass algorithm offers a more optimal and elegant solution, ensuring conditions in a single traversal sequence, thus significantly improving efficiency.

