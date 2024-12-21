# [Leetcode 135: Candy](https://leetcode.com/problems/candy/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two-pass Solution](#approach-2-two-pass-solution)

## Approach 1: Brute Force

### Intuition
In this approach, we continuously update the candies distribution until conditions of relative rankings are satisfied. Start by assigning each child 1 candy. Then iterate over the students multiple times, and if a child has a higher rating than their neighbor, ensure they also have more candies than their neighbor.

### Solution
```csharp
public int Candy(int[] ratings) {
    int n = ratings.Length;
    
    // Initialize each child with 1 candy
    int[] candies = new int[n];
    for (int i = 0; i < n; i++) {
        candies[i] = 1;
    }

    // Flag to check if changes were made in an iteration
    bool changed;
    
    do {
        changed = false;

        // Adjust candies from left to right
        for (int i = 0; i < n - 1; i++) {
            if (ratings[i] < ratings[i + 1] && candies[i] >= candies[i + 1]) {
                candies[i + 1] = candies[i] + 1;
                changed = true;
            }
        }
        
        // Adjust candies from right to left
        for (int i = n - 1; i > 0; i--) {
            if (ratings[i] < ratings[i - 1] && candies[i] >= candies[i - 1]) {
                candies[i - 1] = candies[i] + 1;
                changed = true;
            }
        }
        
    } while (changed);
    
    // Sum up all candies
    int totalCandies = 0;
    for (int candy in candies) {
        totalCandies += candy;
    }

    return totalCandies;
}
```

### Complexity Analysis
- **Time Complexity:** O(n^2) due to potentially making n passes through the array to adjust candies.
- **Space Complexity:** O(n) since an additional candies array of size n is used.

## Approach 2: Two-pass Solution

### Intuition
A more optimal approach leverages two passes to ensure the conditions are satisfied. First, traverse the array from left to right, ensuring each child has more candies than the previous one if their rating is higher. Then, traverse from right to left, ensuring each child has more candies than the next one if their rating is higher. 

### Solution
```csharp
public int Candy(int[] ratings) {
    int n = ratings.Length;
    
    // Initialize each child with 1 candy
    int[] candies = new int[n];
    for (int i = 0; i < n; i++) {
        candies[i] = 1;
    }
    
    // First pass: Left to right
    for (int i = 1; i < n; i++) {
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1;
        }
    }
    
    // Second pass: Right to left
    for (int i = n - 2; i >= 0; i--) {
        if (ratings[i] > ratings[i + 1]) {
            candies[i] = Math.Max(candies[i], candies[i + 1] + 1);
        }
    }
    
    // Sum up all candies
    int totalCandies = 0;
    foreach (int candy in candies) {
        totalCandies += candy;
    }

    return totalCandies;
}
```

### Complexity Analysis
- **Time Complexity:** O(n), as we make two single-pass through the ratings array.
- **Space Complexity:** O(n) since an additional candies array of size n is used.

