## [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

### Approaches
1. [Brute Force](#approach-1-brute-force)
2. [Binary Search](#approach-2-binary-search)

---

### Approach 1: Brute Force

**Intuition**

The brute force approach is fairly straightforward: we start from the smallest possible speed (1 banana per hour) and check for each speed if Koko can finish all the bananas within the allocated hours `H`.

1. Iterate through each possible eating speed `k` from 1 to `max(piles)`, where `max(piles)` represents the maximum bananas in a pile, as this is the fastest speed Koko could need to eat.
2. For each speed, calculate the total hours required to eat all the banana piles. If Koko can finish in `H` hours or less, that speed is a potential solution.
3. Return the smallest such speed.

**Time Complexity:** O(n * m), where `n` is the number of banana piles, and `m` is the maximum number of bananas in any pile.

**Space Complexity:** O(1), as no additional space proportional to input size is used.

**Go Code**

```go
func minEatingSpeed(piles []int, h int) int {
    maxPiles := 0
    for _, pile := range piles {
        if pile > maxPiles {
            maxPiles = pile
        }
    }

    for k := 1; k <= maxPiles; k++ {
        hoursNeeded := 0
        for _, pile := range piles {
            hoursNeeded += (pile + k - 1) / k // Calculate hours needed using ceiling division
        }

        if hoursNeeded <= h {
            return k
        }
    }

    return -1 // if not found, logically impossible
}
```

---

### Approach 2: Binary Search

**Intuition**

Binary search proves to be an optimal strategy since the possible values of Koko's eating speed form a continuous range: Koko can eat from 1 to max(piles) bananas per hour. The goal is to find the smallest feasible speed `k`.

1. Initialize two pointers `low` and `high` representing the range of possible speeds (from 1 to max(piles)).
2. For a given middle speed `mid` between `low` and `high`, determine if Koko can eat all bananas within `H` hours.
3. If Koko can finish the bananas in time, it's possible to check for a smaller `mid` (reduce `high`).
4. Otherwise, increase the `mid` (set `low` to `mid + 1`).
5. The minimum feasible speed will be found when `low` meets `high`.

**Time Complexity:** O(n * log m), where `n` is the number of piles, and `m` is the maximum number of bananas in any pile.

**Space Complexity:** O(1), as no additional space proportional to input size is used.

**Go Code**

```go
func minEatingSpeed(piles []int, h int) int {
    maxPile := 0
    for _, pile := range piles {
        if pile > maxPile {
            maxPile = pile
        }
    }

    low, high := 1, maxPile
    for low < high {
        mid := (low + high) / 2
        if canEatAllBananas(piles, mid, h) {
            high = mid
        } else {
            low = mid + 1
        }
    }
    return low
}

func canEatAllBananas(piles []int, k, h int) bool {
    hours := 0
    for _, pile := range piles {
        hours += (pile + k - 1) / k // Calculate hours needed using ceiling division
        if hours > h {
            return false
        }
    }
    return true
}
```

This approach efficiently narrows down the potential answers to find the minimum speed Koko could manage to eat all bananas in `H` hours using binary search.

