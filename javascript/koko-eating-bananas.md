## [Leetcode 875: Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

### Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Binary Search](#approach-2-binary-search)

---

### Approach 1: Brute Force

#### Intuition
The simplest approach would be to check each possible eating speed from `1` to the maximum number of bananas in one pile. For each speed, calculate the total hours Koko would need to finish all the piles and check if it is within the given timeframe, `H`. This approach ensures that we find the minimum possible speed, as it tries each speed in sequential order.

#### Steps
1. Iterate over all possible eating speeds `k` from `1` to `max(piles)`.
2. For each speed, calculate the total hours needed to finish all piles.
3. If the speed satisfies the time constraint `H`, we have a candidate answer.
4. Return the smallest speed that meets the condition.

```javascript
var minEatingSpeed = function(piles, H) {
    const maxPile = Math.max(...piles); // Find the maximum pile size
    let result = maxPile;
    
    for (let k = 1; k <= maxPile; k++) {
        let totalHours = 0;
        
        for (const pile of piles) {
            totalHours += Math.ceil(pile / k); // Calculate hours needed for each pile
        }
        
        if (totalHours <= H) {
            result = k; // If total hours are within H, update result
            break; // We can break early as we're only interested in the minimum speed
        }
    }
    
    return result;
};
```

- **Time Complexity:** O(max(piles) * n), where `n` is the number of piles.
- **Space Complexity:** O(1) – constant space used.

---

### Approach 2: Binary Search

#### Intuition
The problem of finding the minimal eating speed can be transformed into a search problem on the possible speeds. Instead of testing every speed, we can use binary search to efficiently find the smallest `k` that allows Koko to finish all the bananas in `H` hours. The search space for `k` is from `1` to `max(piles)`. 

#### Steps
1. Use binary search on the range `[1, max(piles)]`.
2. For each middle speed `mid`, calculate the hours needed.
3. If the hours do not exceed `H`, try a smaller speed by setting `end` to `mid - 1`.
4. If the hours exceed `H`, increase the speed by setting `start` to `mid + 1`.
5. Continue until `start` exceeds `end`.
6. The answer will be `start` as the minimum speed that meets the condition.

```javascript
var minEatingSpeed = function(piles, H) {
    const canEatAll = (speed) => {
        let hours = 0;
        for (const pile of piles) {
            hours += Math.ceil(pile / speed); // Calculate hours needed for each pile
        }
        return hours <= H; // Return true if it is possible to eat all bananas in time
    };

    let left = 1;
    let right = Math.max(...piles); // The maximum of our range
    
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        
        if (canEatAll(mid)) {
            right = mid - 1; // Try for a smaller speed
        } else {
            left = mid + 1; // Increase speed
        }
    }
    
    return left; // The result is the left boundary
};
```

- **Time Complexity:** O(n log(max(piles))), where `n` is the number of piles. The log factor comes from the binary search.
- **Space Complexity:** O(1) – constant space used.

This approach is optimal and works efficiently within the given constraints.

