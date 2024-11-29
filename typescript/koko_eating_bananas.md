# 875. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Approach 1: Brute Force (Linear Search on Eating Speed)

### Solution
typescript
```typescript
// Time Complexity: O(n * m), where n is the number of piles and m is the range of eating speeds
// Space Complexity: O(1)

class Solution {
    minEatingSpeed(piles: number[], h: number): number {
        let speed = 1;

        // Increment speed until we find the minimum speed that allows finishing within h hours
        while (!this.canFinish(piles, h, speed)) {
            speed++;
        }

        return speed;
    }

    private canFinish(piles: number[], h: number, speed: number): boolean {
        let hoursNeeded = 0;

        // Calculate the total hours needed at the given speed
        for (let pile of piles) {
            hoursNeeded += Math.ceil(pile / speed);
        }

        return hoursNeeded <= h;
    }
}
```

## Approach 2: Binary Search on Eating Speed (Optimal Solution)

### Solution
typescript
```typescript
// Time Complexity: O(n log m), where n is the number of piles and m is the range of eating speeds
// Space Complexity: O(1)

class Solution {
    minEatingSpeed(piles: number[], h: number): number {
        let start = 1; // Minimum possible eating speed
        let end = this.getMaxPile(piles); // Maximum possible eating speed

        // Perform binary search to find the minimum speed
        while (start < end) {
            let mid = Math.floor(start + (end - start) / 2);
            if (this.canFinish(piles, h, mid)) {
                end = mid; // Narrow the range to the left
            } else {
                start = mid + 1; // Narrow the range to the right
            }
        }

        return start; // Minimum speed to eat all bananas within h hours
    }

    private canFinish(piles: number[], h: number, speed: number): boolean {
        let hoursNeeded = 0;

        // Calculate the total hours needed at the given speed
        for (let pile of piles) {
            hoursNeeded += Math.ceil(pile / speed);
        }

        return hoursNeeded <= h;
    }

    private getMaxPile(piles: number[]): number {
        let max = 0;
        for (let pile of piles) {
            max = Math.max(max, pile); // Find the maximum pile size
        }
        return max;
    }
}
```

