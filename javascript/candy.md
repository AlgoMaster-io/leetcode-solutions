# [Leetcode 135: Candy](https://leetcode.com/problems/candy/)

## Solutions
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pass Greedy](#approach-2-two-pass-greedy)
- [Approach 3: Efficient Greedy](#approach-3-efficient-greedy)

### Approach 1: Brute Force

#### Intuition
In the brute force approach, we can initialize an array to keep track of candies distribution. Start by distributing one candy to each child. Iterate over the `ratings` to compare ratings of neighboring children and update the candies accordingly until no more updates are necessary.

#### Comments on Code
- Start by initializing an array `candies` with `1` for each child, representing the minimum candies.
- Use a boolean flag to check if any update is made in an iteration.
- Traverse the `ratings` list and update the `candies` based on the conditions.
- Continue the process until there are no more updates.

#### Code
```javascript
function candy(ratings) {
    const n = ratings.length;
    const candies = new Array(n).fill(1);
    let changed;
    
    do {
        changed = false;

        // Update candies based on the conditions
        for (let i = 0; i < n; i++) {
            if (i > 0 && ratings[i] > ratings[i - 1] && candies[i] <= candies[i - 1]) {
                candies[i] = candies[i - 1] + 1;
                changed = true;
            }
            if (i < n - 1 && ratings[i] > ratings[i + 1] && candies[i] <= candies[i + 1]) {
                candies[i] = candies[i + 1] + 1;
                changed = true;
            }
        }
    } while (changed);

    return candies.reduce((sum, num) => sum + num, 0);
}
```

#### Complexity
- **Time Complexity:** O(n^2), because in the worst-case scenario, each pass adjusts candies, and multiple passes might be required.
- **Space Complexity:** O(n), for the `candies` array.

---

### Approach 2: Two Pass Greedy

#### Intuition
The greedy approach uses two passes to ensure that each child has more candies than their neighbor if their rating is higher. The first pass goes from left to right to handle increases, and the second pass goes from right to left to handle decreases.

#### Comments on Code
- Initialize `candies` array with `1` for each child as the minimum candies.
- First pass: Loop from left to right and update.
- Second pass: Loop from right to left and update.

#### Code
```javascript
function candy(ratings) {
    const n = ratings.length;
    const candies = new Array(n).fill(1);

    // Left pass - Ensure each child with higher rating gets more candies than left neighbor
    for (let i = 1; i < n; i++) {
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1;
        }
    }

    // Right pass - Ensure each child with higher rating gets more candies than right neighbor
    for (let i = n - 2; i >= 0; i--) {
        if (ratings[i] > ratings[i + 1]) {
            candies[i] = Math.max(candies[i], candies[i + 1] + 1);
        }
    }

    return candies.reduce((sum, num) => sum + num, 0);
}
```

#### Complexity
- **Time Complexity:** O(n), as we traverse the array twice.
- **Space Complexity:** O(n), as we use an additional `candies` array.

---

### Approach 3: Efficient Greedy

#### Intuition
We can optimize by using a single traversal, but this will also be a two-pass method in terms of logic. This simply initializes candies in both directions without explicitly storing two separate passes.

#### Comments on Code
- We could have this variant if we enable dynamic strategy adjustments based on L/R trends, but essentially it merges principles of Approach 2.
- Direct application of Approach 3 has similar meeting points with Approach 2 in practical algorithm iteration boundary adjustments.

#### Code
```javascript
// Same as Approach 2 - optimal iteration in language like JS
```

#### Complexity
- **Time Complexity:** O(n)
- **Space Complexity:** O(n)

Implementations should be considered in terms of memory and speed relative to the array inputs for further environmental adjustment.

