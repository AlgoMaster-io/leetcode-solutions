# 875. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Approach 1: Brute Force (Linear Search on Eating Speed)

### Solution
```javascript
// Time Complexity: O(n * m), where n is the number of piles and m is the range of eating speeds
// Space Complexity: O(1)
function minEatingSpeed(piles, h) {
    let speed = 1;

    // Increment speed until we find the minimum speed that allows finishing within h hours
    while (!canFinish(piles, h, speed)) {
        speed++;
    }

    return speed;
}

function canFinish(piles, h, speed) {
    let hoursNeeded = 0;

    // Calculate the total hours needed at the given speed
    for (let pile of piles) {
        hoursNeeded += Math.ceil(pile / speed);
    }

    return hoursNeeded <= h;
}
```

## Approach 2: Binary Search on Eating Speed (Optimal Solution)

### Solution
```javascript
// Time Complexity: O(n log m), where n is the number of piles and m is the range of eating speeds
// Space Complexity: O(1)
function minEatingSpeed(piles, h) {
    let start = 1; // Minimum possible eating speed
    let end = getMaxPile(piles); // Maximum possible eating speed

    // Perform binary search to find the minimum speed
    while (start < end) {
        let mid = Math.floor(start + (end - start) / 2);
        if (canFinish(piles, h, mid)) {
            end = mid; // Narrow the range to the left
        } else {
            start = mid + 1; // Narrow the range to the right
        }
    }

    return start; // Minimum speed to eat all bananas within h hours
}

function canFinish(piles, h, speed) {
    let hoursNeeded = 0;

    // Calculate the total hours needed at the given speed
    for (let pile of piles) {
        hoursNeeded += Math.ceil(pile / speed);
    }

    return hoursNeeded <= h;
}

function getMaxPile(piles) {
    return Math.max(...piles); // Find the maximum pile size
}
```

