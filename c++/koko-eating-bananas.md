# [Leetcode 875: Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Navigation
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Binary Search](#approach-2-binary-search)

### Approach 1: Brute Force

**Intuition:**

In the brute force approach, we can start checking for each possible speed from 1 upwards to infinity and calculate the total hours required to eat all bananas at this speed. The minimum speed at which Koko can eat all bananas within `h` hours will be our answer.

**Algorithm:**

1. Start with an initial speed variable `k` set to 1.
2. For each speed, calculate how many hours it would take to finish all piles.
3. For a given speed `k`, for each pile of size `piles[i]`, the required hours will be `ceil(piles[i] / k)`.
4. If at any speed `k`, the total calculated hours is less than or equal to `h`, then this is the minimum speed needed.
5. Otherwise, increment the speed and try again.

This approach is not optimal since it may require checking many potential speeds, especially if the maximum number of bananas per hour is large.

**Code:**

```cpp
#include <vector>
#include <cmath>

bool canEatAll(std::vector<int>& piles, int k, int h) {
    int totalHours = 0;
    for (int bananas : piles) {
        // Calculate hours needed for this pile at speed k
        totalHours += std::ceil((double)bananas / k);
        if (totalHours > h) return false;  // early termination if we exceed the given hours
    }
    return totalHours <= h;
}

int minEatingSpeed(std::vector<int>& piles, int h) {
    int k = 1;
    while (true) {
        if (canEatAll(piles, k, h)) return k;  // found the minimum speed
        ++k;
    }
}
```

**Time Complexity:**

- O(max(piles) * n), where `n` is the number of piles. This represents the worst-case scenario when we have to check each speed incrementally up to the maximum bananas in any pile.

**Space Complexity:**

- O(1), since we are only using a fixed amount of additional memory.

### Approach 2: Binary Search

**Intuition:**

The optimal solution utilizes binary search to efficiently find the minimum possible speed at which Koko can finish eating all bananas within `h` hours. The key is understanding that a feasible solution exists within the range of speeds `[1, max(piles)]`. By applying a binary search strategy, we can reduce unnecessary checks over the potential speeds.

**Algorithm:**

1. Initialize `low` to 1 and `high` to the maximum number of bananas from the piles.
2. While `low` is less than or equal to `high`:
   - Calculate `mid` as `(low + high) / 2`.
   - Check if Koko can eat all the bananas at speed `mid` using a helper function.
   - If yes, then try to minimize the speed further by setting `high = mid - 1`.
   - If not, increase the speed by setting `low = mid + 1`.
3. The minimal speed found will be stored in `low`.

**Code:**

```cpp
#include <vector>
#include <cmath>

bool canFinish(std::vector<int>& piles, int speed, int h) {
    int totalHours = 0;
    for (int bananas : piles) {
        // Calculate hours needed for this pile at speed
        totalHours += std::ceil((double)bananas / speed);
        if (totalHours > h) return false; // exceeding allowed hours
    }
    return totalHours <= h;
}

int minEatingSpeed(std::vector<int>& piles, int h) {
    int low = 1;
    int high = *max_element(piles.begin(), piles.end());
    
    while (low <= high) {
        int mid = low + (high - low) / 2; // mid speed
        if (canFinish(piles, mid, h)) {
            high = mid - 1; // try for a smaller speed
        } else {
            low = mid + 1; // increase speed
        }
    }
    return low;
}
```

**Time Complexity:**

- O(n log(max(piles))), where `n` is the number of piles. The log factor comes from the binary search and `n` from calculating hours.

**Space Complexity:**

- O(1), as we are not using any additional data structures that grow with input size.

