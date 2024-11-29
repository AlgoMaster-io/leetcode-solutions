# 875. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Approach 1: Brute Force (Linear Search on Eating Speed)

### Solution
csharp
```csharp
// Time Complexity: O(n * m), where n is the number of piles and m is the range of eating speeds
// Space Complexity: O(1)
public class Solution {
    public int MinEatingSpeed(int[] piles, int h) {
        int speed = 1;

        // Increment speed until we find the minimum speed that allows finishing within h hours
        while (!CanFinish(piles, h, speed)) {
            speed++;
        }

        return speed;
    }

    private bool CanFinish(int[] piles, int h, int speed) {
        int hoursNeeded = 0;

        // Calculate the total hours needed at the given speed
        foreach (int pile in piles) {
            hoursNeeded += (pile + speed - 1) / speed; // Equivalent to Math.Ceiling((double)pile / speed)
        }

        return hoursNeeded <= h;
    }
}
```

## Approach 2: Binary Search on Eating Speed (Optimal Solution)

### Solution
csharp
```csharp
// Time Complexity: O(n log m), where n is the number of piles and m is the range of eating speeds
// Space Complexity: O(1)
public class Solution {
    public int MinEatingSpeed(int[] piles, int h) {
        int start = 1; // Minimum possible eating speed
        int end = GetMaxPile(piles); // Maximum possible eating speed

        // Perform binary search to find the minimum speed
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (CanFinish(piles, h, mid)) {
                end = mid; // Narrow the range to the left
            } else {
                start = mid + 1; // Narrow the range to the right
            }
        }

        return start; // Minimum speed to eat all bananas within h hours
    }

    private bool CanFinish(int[] piles, int h, int speed) {
        int hoursNeeded = 0;

        // Calculate the total hours needed at the given speed
        foreach (int pile in piles) {
            hoursNeeded += (pile + speed - 1) / speed; // Equivalent to Math.Ceiling((double)pile / speed)
        }

        return hoursNeeded <= h;
    }

    private int GetMaxPile(int[] piles) {
        int max = 0;
        foreach (int pile in piles) {
            max = Math.Max(max, pile); // Find the maximum pile size
        }
        return max;
    }
}
```

