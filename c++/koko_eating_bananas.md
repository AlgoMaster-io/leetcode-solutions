# 875. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Approach 1: Brute Force (Linear Search on Eating Speed)

### Solution
```cpp
// Time Complexity: O(n * m), where n is the number of piles and m is the range of eating speeds
// Space Complexity: O(1)
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int speed = 1;

        // Increment speed until we find the minimum speed that allows finishing within h hours
        while (!canFinish(piles, h, speed)) {
            speed++;
        }

        return speed;
    }

private:
    bool canFinish(const vector<int>& piles, int h, int speed) {
        int hoursNeeded = 0;

        // Calculate the total hours needed at the given speed
        for (int pile : piles) {
            hoursNeeded += (pile + speed - 1) / speed; // Equivalent to ceil(pile / speed)
        }

        return hoursNeeded <= h;
    }
};
```

## Approach 2: Binary Search on Eating Speed (Optimal Solution)

### Solution
```cpp
// Time Complexity: O(n log m), where n is the number of piles and m is the range of eating speeds
// Space Complexity: O(1)
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int start = 1; // Minimum possible eating speed
        int end = getMaxPile(piles); // Maximum possible eating speed

        // Perform binary search to find the minimum speed
        while (start < end) {
            int mid = start + (end - start) / 2;
            if (canFinish(piles, h, mid)) {
                end = mid; // Narrow the range to the left
            } else {
                start = mid + 1; // Narrow the range to the right
            }
        }

        return start; // Minimum speed to eat all bananas within h hours
    }

private:
    bool canFinish(const vector<int>& piles, int h, int speed) {
        int hoursNeeded = 0;

        // Calculate the total hours needed at the given speed
        for (int pile : piles) {
            hoursNeeded += (pile + speed - 1) / speed; // Equivalent to ceil(pile / speed)
        }

        return hoursNeeded <= h;
    }

    int getMaxPile(const vector<int>& piles) {
        int maxPile = 0;
        for (int pile : piles) {
            maxPile = max(maxPile, pile); // Find the maximum pile size
        }
        return maxPile;
    }
};
```

