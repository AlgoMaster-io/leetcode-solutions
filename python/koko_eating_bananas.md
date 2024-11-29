# 875. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Approach 1: Brute Force (Linear Search on Eating Speed)

### Solution
```python
# Time Complexity: O(n * m), where n is the number of piles and m is the range of eating speeds
# Space Complexity: O(1)
class Solution:
    def minEatingSpeed(self, piles, h):
        speed = 1

        # Increment speed until we find the minimum speed that allows finishing within h hours
        while not self.canFinish(piles, h, speed):
            speed += 1

        return speed

    def canFinish(self, piles, h, speed):
        hoursNeeded = 0

        # Calculate the total hours needed at the given speed
        for pile in piles:
            hoursNeeded += (pile + speed - 1) // speed  # Equivalent to math.ceil(pile / speed)

        return hoursNeeded <= h
```

## Approach 2: Binary Search on Eating Speed (Optimal Solution)

### Solution
```python
# Time Complexity: O(n log m), where n is the number of piles and m is the range of eating speeds
# Space Complexity: O(1)
class Solution:
    def minEatingSpeed(self, piles, h):
        start = 1  # Minimum possible eating speed
        end = self.getMaxPile(piles)  # Maximum possible eating speed

        # Perform binary search to find the minimum speed
        while start < end:
            mid = start + (end - start) // 2
            if self.canFinish(piles, h, mid):
                end = mid  # Narrow the range to the left
            else:
                start = mid + 1  # Narrow the range to the right

        return start  # Minimum speed to eat all bananas within h hours

    def canFinish(self, piles, h, speed):
        hoursNeeded = 0

        # Calculate the total hours needed at the given speed
        for pile in piles:
            hoursNeeded += (pile + speed - 1) // speed  # Equivalent to math.ceil(pile / speed)

        return hoursNeeded <= h

    def getMaxPile(self, piles):
        return max(piles)  # Find the maximum pile size
```

