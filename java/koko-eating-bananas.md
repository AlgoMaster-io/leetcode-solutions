# [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Table of Contents
- [Approach 1: Brute Force Solution](#approach-1-brute-force-solution)
- [Approach 2: Binary Search Solution](#approach-2-binary-search-solution)

---

## Approach 1: Brute Force Solution

### Intuition:
In this approach, we try every possible eating speed `k` starting from 1 until we find a speed that allows Koko to eat all the bananas within `h` hours. For each speed, we simulate the eating process and check if Koko can finish all bananas within `h` hours.

### Solution:

```java
public class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int maxPile = 0;
        for (int pile : piles) {
            maxPile = Math.max(maxPile, pile); // Find the maximum number of bananas in a pile
        }
        
        // Try each possible speed from 1 to maxPile
        for (int speed = 1; speed <= maxPile; speed++) {
            if (canEatAllBananas(piles, h, speed)) {
                return speed; // Return the first speed where Koko can eat all bananas in time
            }
        }
        
        return maxPile; // In the worst case, return maxPile which is 1 hour per pile
    }
    
    private boolean canEatAllBananas(int[] piles, int h, int k) {
        int hoursNeeded = 0;
        for (int pile : piles) {
            hoursNeeded += (int) Math.ceil((double) pile / k); // Calculate hours needed at this speed
        }
        return hoursNeeded <= h;
    }
}
```

### Time Complexity:
- We may have to try all speeds from 1 to the maximum pile size, leading to `O(max(piles) * n)`, where `n` is the number of piles.

### Space Complexity:
- `O(1)`, no additional space other than input and local variables.

---

## Approach 2: Binary Search Solution

### Intuition:
Using binary search, we efficiently narrow down the range of Koko's possible eating speeds. Instead of checking each speed one by one, we continuously adjust our search range based on whether the current mid speed allows Koko to eat all bananas in `h` hours. The goal is to find the minimal speed `k` that works.

### Solution:

```java
public class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int maxPile = 0;
        for (int pile : piles) {
            maxPile = Math.max(maxPile, pile); // Determine the biggest pile for upper bound of search
        }
        
        int left = 1; // Minimum possible speed
        int right = maxPile; // Maximum possible speed
        int result = maxPile;
        
        while (left <= right) {
            int mid = left + (right - left) / 2; // Mid-point speed
            if (canEatAllBananas(piles, h, mid)) {
                result = mid; // If Koko can eat all, try a smaller speed
                right = mid - 1;
            } else {
                left = mid + 1; // Otherwise, increase the speed
            }
        }
        
        return result;
    }
    
    private boolean canEatAllBananas(int[] piles, int h, int k) {
        int hoursNeeded = 0;
        for (int pile : piles) {
            hoursNeeded += (int) Math.ceil((double) pile / k); // Calculate hours needed at this speed
        }
        return hoursNeeded <= h;
    }
}
```

### Time Complexity:
- `O(n log(max(piles)))`, where `log(max(piles))` is for the binary search and `n` for each check of `canEatAllBananas`.

### Space Complexity:
- `O(1)`, using only a constant amount of extra space.

By leveraging binary search, this approach is significantly more efficient than brute force and ideal for larger inputs.

