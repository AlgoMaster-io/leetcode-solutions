# [Candy - LeetCode Problem 135](https://leetcode.com/problems/candy/)

## Approaches

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two-pass Linear Greedy](#approach-2-two-pass-linear-greedy)

### Approach 1: Brute Force

#### Intuition
The problem can be approached with a brute-force method where we repeatedly iterate over the array to ensure all conditions are satisfied: each child gets at least one candy, and children with a higher rating than their neighbors get more candy than their neighbors. This involves iteratively adjusting the candy values until we reach a stable state.

#### Code
```java
public int candy(int[] ratings) {
    int n = ratings.length;
    
    // Initial allocation of 1 candy for each child
    int[] candies = new int[n];
    for (int i = 0; i < n; i++) {
        candies[i] = 1;
    }
    
    boolean hasChanged = true;
    while (hasChanged) {
        hasChanged = false;
        
        // Traverse the array from left to right.
        for (int i = 0; i < n - 1; i++) {
            if (ratings[i] < ratings[i + 1] && candies[i] >= candies[i + 1]) {
                candies[i + 1] = candies[i] + 1;
                hasChanged = true;
            }
        }
        
        // Traverse the array from right to left.
        for (int i = n - 1; i > 0; i--) {
            if (ratings[i] < ratings[i - 1] && candies[i] >= candies[i - 1]) {
                candies[i - 1] = candies[i] + 1;
                hasChanged = true;
            }
        }
    }
    
    // Sum up candies
    int sum = 0;
    for (int candy : candies) {
        sum += candy;
    }
    return sum;
}
```

#### Time Complexity
- **Time**: O(n^2) in the worst case due to the repeated iteration over the array.
- **Space**: O(n) for the `candies` array.

### Approach 2: Two-pass Linear Greedy

#### Intuition
This approach improves upon the brute force by using a two-pass linear greedy strategy. We handle conditions in a more organized manner:

1. Make one pass from left to right, ensuring children with a higher rating than the previous one get more candies.
2. Make a second pass from right to left, doing similar adjustments but also ensuring not to disturb what was established in the first pass.

#### Code
```java
public int candy(int[] ratings) {
    int n = ratings.length;
    if (n <= 1) return n;
    
    // Create an array to store the number of candies each child should get
    int[] candies = new int[n];
    // Each child gets at least one candy.
    Arrays.fill(candies, 1);
    
    // Traverse the ratings from left to right.
    for (int i = 1; i < n; i++) {
        // If the current child has a higher rating than the previous one
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1;
        }
    }
    
    // Traverse the ratings from right to left.
    for (int i = n - 2; i >= 0; i--) {
        // If the current child has a higher rating than the next one
        if (ratings[i] > ratings[i + 1]) {
            candies[i] = Math.max(candies[i], candies[i + 1] + 1);
        }
    }
    
    // Sum up candies
    int sum = 0;
    for (int candy : candies) {
        sum += candy;
    }
    return sum;
}
```

#### Time Complexity
- **Time**: O(n), where n is the number of children, because each array is traversed twice.
- **Space**: O(n) for the `candies` array.

