# [Leetcode 875: Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Solutions

1. [Brute Force Approach](#brute-force-approach)
2. [Binary Search Approach](#binary-search-approach)

### Brute Force Approach

**Intuition:**

The simplest approach is to iterate over all potential eating speeds from `1` to the maximum number of bananas in any pile. For each speed, we calculate the total number of hours required for Koko to eat all the bananas. This involves iterating over each pile and determining how many hours it would take Koko to eat the bananas in that pile at the current speed. We aim to find the minimum speed where the total hours do not exceed `h`.

**Steps:**

1. Iterate over each possible eating speed from `1` to the max pile size.
2. For each speed, calculate the total hours needed to eat all bananas.
3. Return the first speed that allows Koko to finish within `h` hours.

**Time Complexity:** O(n * m), where `n` is the number of piles and `m` is the maximum number of bananas in a pile.

**Space Complexity:** O(1)

```csharp
public class Solution {
    public int MinEatingSpeed(int[] piles, int h) {
        int maxPile = 0;
        // Find the maximum pile size
        foreach (var pile in piles) {
            maxPile = Math.Max(maxPile, pile);
        }

        // Iterate over all possible speeds
        for (int speed = 1; speed <= maxPile; speed++) {
            int hours = 0;
            foreach (var pile in piles) {
                // Calculate the number of hours needed for this pile
                hours += (pile + speed - 1) / speed; // equivalent to Math.Ceiling(pile / speed)
            }

            // Check if within limit of hours
            if (hours <= h) {
                return speed;
            }
        }

        return maxPile; // corner case, should never be here if input constraints are valid
    }
}
```

### Binary Search Approach

**Intuition:**

The binary search approach is more efficient. Instead of testing each possible eating speed, we use binary search to find the minimum speed. The key insight is that if a speed `k` works (Koko can finish eating within `h`), then any speed greater than `k` will also work. Thus, we perform a binary search over the possible speed values.

**Steps:**

1. Define the search range from `1` (minimum possible speed) to `maxPile` (maximum bananas in any pile).
2. While low <= high, calculate the middle speed, and test if it's possible to eat all bananas within `h` hours.
3. If possible, try a smaller speed (adjust the end of the range).
4. If not possible, increase the speed (adjust the start of the range).

**Time Complexity:** O(n log m), where `n` is the number of piles and `m` is the maximum number of bananas in a pile.

**Space Complexity:** O(1)

```csharp
public class Solution {
    public int MinEatingSpeed(int[] piles, int h) {
        int left = 1, right = 0;

        // Find the maximum pile size
        foreach (var pile in piles) {
            right = Math.Max(right, pile);
        }

        // Start binary search
        while (left < right) {
            int mid = left + (right - left) / 2;
            int hours = 0;

            foreach (var pile in piles) {
                // Calculate the number of hours needed at mid speed
                hours += (pile + mid - 1) / mid;
            }

            if (hours <= h) {
                // If within limit, try slower speed
                right = mid;
            } else {
                // Otherwise, increase speed
                left = mid + 1;
            }
        }

        return left;
    }
}
```

In this binary search approach, we efficiently find the minimum feasible speed by halving the search space in each step. This is significantly faster than the brute force approach, especially for large inputs.

