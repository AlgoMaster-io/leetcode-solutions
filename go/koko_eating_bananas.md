# 875. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Approach 1: Brute Force (Linear Search on Eating Speed)

### Solution
go
```go
// Time Complexity: O(n * m), where n is the number of piles and m is the range of eating speeds
// Space Complexity: O(1)
package main

func minEatingSpeed(piles []int, h int) int {
    speed := 1

    // Increment speed until we find the minimum speed that allows finishing within h hours
    for !canFinish(piles, h, speed) {
        speed++
    }

    return speed
}

func canFinish(piles []int, h int, speed int) bool {
    hoursNeeded := 0

    // Calculate the total hours needed at the given speed
    for _, pile := range piles {
        hoursNeeded += (pile + speed - 1) / speed // Equivalent to Math.ceil(pile / speed)
    }

    return hoursNeeded <= h
}
```

## Approach 2: Binary Search on Eating Speed (Optimal Solution)

### Solution
go
```go
// Time Complexity: O(n log m), where n is the number of piles and m is the range of eating speeds
// Space Complexity: O(1)
package main

func minEatingSpeed(piles []int, h int) int {
    start, end := 1, getMaxPile(piles) // Minimum and maximum possible eating speeds

    // Perform binary search to find the minimum speed
    for start < end {
        mid := start + (end-start)/2
        if canFinish(piles, h, mid) {
            end = mid // Narrow the range to the left
        } else {
            start = mid + 1 // Narrow the range to the right
        }
    }

    return start // Minimum speed to eat all bananas within h hours
}

func canFinish(piles []int, h int, speed int) bool {
    hoursNeeded := 0

    // Calculate the total hours needed at the given speed
    for _, pile := range piles {
        hoursNeeded += (pile + speed - 1) / speed // Equivalent to Math.ceil(pile / speed)
    }

    return hoursNeeded <= h
}

func getMaxPile(piles []int) int {
    max := 0
    for _, pile := range piles {
        if pile > max {
            max = pile // Find the maximum pile size
        }
    }
    return max
}
```

