# [Leetcode 135: Candy](https://leetcode.com/problems/candy/)

## Approaches
- [Approach 1: Brute Force with Multiple Passes](#approach-1-brute-force-with-multiple-passes)
- [Approach 2: Greedy Single Pass with Two Arrays](#approach-2-greedy-single-pass-with-two-arrays)
- [Approach 3: Greedy In-place Optimization](#approach-3-greedy-in-place-optimization)

## Approach 1: Brute Force with Multiple Passes

### Intuition
The problem can be approached by distributing candies such that each child has more than their neighbors if they possess a higher rating. A simple but inefficient solution is repeatedly iterating over the ratings and adjusting the candy distribution each time a rule is violated until no further changes are needed.

### Steps
1. Initialize an array `candies` where each child gets at least one candy.
2. Repeatedly iterate through the ratings list:
   - For each pair of neighbors, if a child has a higher rating but not more candies, increase the candies for that child.
3. Continue iterating until a complete pass without changes is achieved.

```python
def candy(ratings):
    n = len(ratings)
    candies = [1] * n
    changed = True

    while changed:
        changed = False
        # Forward pass
        for i in range(n - 1):
            if ratings[i] < ratings[i + 1] and candies[i] >= candies[i + 1]:
                candies[i + 1] = candies[i] + 1
                changed = True
        # Backward pass
        for i in range(n - 1, 0, -1):
            if ratings[i] < ratings[i - 1] and candies[i] >= candies[i - 1]:
                candies[i - 1] = candies[i] + 1
                changed = True

    return sum(candies)
```

### Time and Space Complexity
- **Time Complexity**: O(n^2) in the worst case due to continuous forward and backward passes.
- **Space Complexity**: O(n) for the candies array.

## Approach 2: Greedy Single Pass with Two Arrays

### Intuition
Instead of iterating until stabilization, we can fix one direction then the other. First, ensure that each child has more candies than its left neighbor with higher ratings, then correct them if the right condition is violated.

### Steps
1. Create two arrays to record the minimum candies from the left and right perspectives (`left` and `right`).
2. Traverse from left to right, ensuring each child has one more candy than the left child if they have a higher rating.
3. Repeat similarly from right to left.
4. The final candy count for each child will be the maximum of the two arrays.

```python
def candy(ratings):
    n = len(ratings)
    left = [1] * n
    right = [1] * n
    
    # Calculate left to right
    for i in range(1, n):
        if ratings[i] > ratings[i - 1]:
            left[i] = left[i - 1] + 1

    # Calculate right to left
    for i in range(n - 2, -1, -1):
        if ratings[i] > ratings[i + 1]:
            right[i] = right[i + 1] + 1

    # Calculate the total candies needed
    candies = 0
    for i in range(n):
        candies += max(left[i], right[i])

    return candies
```

### Time and Space Complexity
- **Time Complexity**: O(n) as each list is traversed twice.
- **Space Complexity**: O(n) for the two auxiliary arrays.

## Approach 3: Greedy In-place Optimization

### Intuition
Combine the idea of two directional passes into a single array to minimize space usage. Instead of separate arrays for left and right, modify a single array as you go.

### Steps
1. Initialize the `candies` array with `1` for each child.
2. Two passes:
   - Forward to ensure rating increasing to the right gets more candies.
   - Backward to ensure rating increasing to the left gets more candies while maintaining the conditions.

```python
def candy(ratings):
    n = len(ratings)
    candies = [1] * n

    # Forward pass
    for i in range(1, n):
        if ratings[i] > ratings[i - 1]:
            candies[i] = candies[i - 1] + 1

    # Backward pass
    for i in range(n - 2, -1, -1):
        if ratings[i] > ratings[i + 1]:
            candies[i] = max(candies[i], candies[i + 1] + 1)

    return sum(candies)
```

### Time and Space Complexity
- **Time Complexity**: O(n) as there's a forward and backward pass.
- **Space Complexity**: O(n) for the candy array used for computation.

Each approach provides a progressively optimized solution, from a brute force approach to a more efficient greedy strategy using single array manipulation.

