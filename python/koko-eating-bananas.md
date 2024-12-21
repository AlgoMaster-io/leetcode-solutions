# [Leetcode Problem 875: Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

## Approaches:
- [Approach 1: Linear Search](#approach-1-linear-search)
- [Approach 2: Binary Search](#approach-2-binary-search)

---

## Approach 1: Linear Search

### Intuition:
The simplest approach to solve this problem is to iteratively try each possible eating speed `k` starting from `1` and check if Koko can eat all bananas within the given `h` hours. For each speed, we need to calculate how many hours it would take for Koko to finish all the bananas and see if it is less than or equal to `h`.

### Steps:
1. Start from the smallest possible eating rate `k = 1`.
2. For each `k`, calculate the total hours Koko needs to eat all the bananas.
3. If the total hours are less than or equal to `h`, that means `k` is a valid speed.
4. The first valid `k` is our answer.

### Code:
```python
def minEatingSpeed(piles, h):
    def canEatAllBananas(k):
        hours = 0
        for pile in piles:
            # Calculate the hours needed to finish this pile with eating speed k
            hours += -(-pile // k)  # same as math.ceil(pile / k)
        return hours <= h
    
    # Start from the minimum possible speed, which is 1
    k = 1
    while True:
        if canEatAllBananas(k):
            break
        k += 1
    return k
```

### Complexity:
- **Time Complexity**: O(max(piles) * n), where n is the length of piles. In the worst case, you'd have to try each rate up to the maximum number of bananas in one pile.
- **Space Complexity**: O(1), as we are only using a constant amount of extra space.

---

## Approach 2: Binary Search

### Intuition:
Instead of checking each speed one by one, we can leverage binary search to efficiently find the minimum valid speed. The idea is to adjust our search space based on whether a particular speed allows Koko to finish the bananas in time.

### Steps:
1. Define the range for binary search: start from `1` (minimum speed) to `max(piles)` (maximum speed).
2. For each potential speed `mid`, calculate the total hours needed.
3. If Koko can finish the bananas with this speed within `h` hours, try a smaller speed. Otherwise, try a larger speed.
4. Repeat the process until the search space is reduced and the optimal speed is found.

### Code:
```python
def minEatingSpeed(piles, h):
    def canEatAllBananas(k):
        hours = 0
        for pile in piles:
            # Calculate the hours needed to finish this pile with eating speed k
            hours += -(-pile // k)  # same as math.ceil(pile / k)
        return hours <= h
    
    # Define search boundaries
    left, right = 1, max(piles)
    
    # Perform binary search
    while left < right:
        mid = (left + right) // 2
        if canEatAllBananas(mid):
            right = mid
        else:
            left = mid + 1
    
    return left
```

### Complexity:
- **Time Complexity**: O(n log(max(piles))), where n is the length of piles. The binary search reduces the potential speed range logarithmically.
- **Space Complexity**: O(1), as we only use a constant amount of additional space.

Thus, the binary search approach is both time-efficient and space-efficient, offering a significant improvement over the linear search method.

