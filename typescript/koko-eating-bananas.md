[Leetcode 875: Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Binary Search](#approach-2-binary-search)

### Approach 1: Brute Force

#### Intuition:
The brute force approach involves trying every possible integer speed from 1 to the maximum pile size (since Koko can't eat faster than the largest pile if she finishes within H hours). For each speed, calculate the total number of hours required to eat all bananas and check if it's within the given limit. Start from speed 1 and continue until we find a solution.

#### Steps:
1. Initialize `minHours` to a large number representing the minimum hours needed.
2. For every possible eating speed `k` from 1 to `max(piles)`, calculate the total hours Koko needs to finish all piles.
3. For each pile, compute the time needed as `Math.ceil(pile / k)`.
4. Sum these times for all piles and store the minimum hours for which Koko can eat all within H hours.
5. Return the smallest such k.

```typescript
function minEatingSpeed(piles: number[], h: number): number {
    let maxPile = Math.max(...piles);
    let minK = maxPile;

    for (let k = 1; k <= maxPile; k++) {
        let hours = 0;
        for (const pile of piles) {
            // Calculate hours needed for this pile with speed k
            hours += Math.ceil(pile / k);
        }
        if (hours <= h) {
            minK = Math.min(minK, k);
        }
    }
    return minK;
}
```

**Time Complexity**: O(n * m) where n is the number of piles and m is the largest pile size.<br>
**Space Complexity**: O(1), we are using a constant amount of extra space.

### Approach 2: Binary Search

#### Intuition:
Using a binary search allows us to efficiently find the minimum eating speed by leveraging the ordered nature of possible speeds. Instead of checking each integer speed sequentially, divide the range of potential speeds and concentrate on the half where the solution could exist until a valid speed is found.

#### Steps:
1. Initialize `left` as 1 and `right` as `max(piles)`.
2. While `left` is less than or equal to `right`:
   - Calculate `mid` as the average of `left` and `right`.
   - Compute the total hours required with speed `mid`.
   - If hours are less than or equal to`h`, store `mid` as a potential solution and adjust `right` to `mid - 1` to find a potentially smaller valid speed.
   - Otherwise, adjust `left` to `mid + 1` since `mid` is too slow.
3. Return the smallest speed found.

```typescript
function minEatingSpeed(piles: number[], h: number): number {
    let left = 1;
    let right = Math.max(...piles);
    let result = right;
    
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        let hours = 0;
        
        for (const pile of piles) {
            // Calculate time to eat the current pile with speed mid
            hours += Math.ceil(pile / mid);
        }

        if (hours <= h) {
            // Found a valid speed, update the result and search for potentially smaller speeds
            result = mid;
            right = mid - 1;
        } else {
            // mid is too slow, try faster speeds
            left = mid + 1;
        }
    }
    return result;
}
```

**Time Complexity**: O(n log m), where n is the number of piles and m is the largest pile size. The binary search complexity is log m and we iterate over piles for each step.<br>
**Space Complexity**: O(1), using a constant amount of extra space.

### Conclusion
The brute force approach should only be used for illustrative purposes or small test cases due to its inefficiency with larger inputs. The binary search approach is optimal and recommended for an efficient solution.

