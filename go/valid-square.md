# [Leetcode 593: Valid Square](https://leetcode.com/problems/valid-square/)

## Approaches
- [Approach 1: Brute Force Using Distance Formula](#approach-1-brute-force-using-distance-formula)
- [Approach 2: Optimized with Sorting and HashSet](#approach-2-optimized-with-sorting-and-hashset)

## Approach 1: Brute Force Using Distance Formula

### Intuition
To determine if four points form a square, they must define two pairs of equal-length sides (the sides of the square) and one pair of equal-length diagonals. The simplest way is to calculate the distance between every pair of points and ensure they satisfy the criteria above.

### Steps
1. Calculate the squared distances between all pairs of points to avoid dealing with floating-point precision.
2. Use these distances to verify that there are two distinct distances: 
   - Validate that there are four instances of the smaller distance (sides of the square).
   - Validate that there are two instances of the larger distance (the diagonals of the square).

Here's the GO code for the brute force approach:

```go
import "math"

func validSquare(p1 []int, p2 []int, p3 []int, p4 []int) bool {
    dist := func(p1, p2 []int) int {
        return (p1[0]-p2[0])*(p1[0]-p2[0]) + (p1[1]-p2[1])*(p1[1]-p2[1])
    }

    d := []int{
        dist(p1, p2),
        dist(p1, p3),
        dist(p1, p4),
        dist(p2, p3),
        dist(p2, p4),
        dist(p3, p4),
    }

    sort.Ints(d)

    // Check that we have four equal smaller distances and two equal larger distances
    return d[0] > 0 && d[0] == d[1] && d[1] == d[2] && d[2] == d[3] && d[4] == d[5]
}
```

### Time Complexity
- The function performs a constant number of operations involving distance calculations, making the time complexity O(1).

### Space Complexity
- The space complexity is O(1) since we're using a fixed number of variables.

## Approach 2: Optimized with Sorting and HashSet

### Intuition
We can enhance this approach by strictly leveraging the uniqueness and orderings provided by sets and sorting mechanisms. We check precisely for two different distance values, which represent the side and diagonal of a square when sorted by length.

### Steps
1. Compute all pairwise squared distances between the points.
2. Insert these distances into a set to automatically handle duplicates.
3. The set should have exactly two distinct elements, representing the side and diagonal lengths.

Here's the GO code for the optimized approach:

```go
import "sort"

func validSquare(p1 []int, p2 []int, p3 []int, p4 []int) bool {
    dist := func(p1, p2 []int) int {
        return (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1])
    }
    
    // Calculate squared distances
    dists := []int{
        dist(p1, p2),
        dist(p1, p3),
        dist(p1, p4),
        dist(p2, p3),
        dist(p2, p4),
        dist(p3, p4),
    }
    
    // Sort distances
    sort.Ints(dists)
    
    // Check for square conditions: four equal smallest distances and two equal larger distances
    return dists[0] > 0 && dists[0] == dists[1] && dists[1] == dists[2] && dists[2] == dists[3] && dists[4] == dists[5]
}
```

### Time Complexity
- The constants due to the small size make it O(1), however notationally it's O(n log n) for sorting six distances.

### Space Complexity
- The space complexity remains O(1) because we store a fixed count of distances.

Both approaches ensure the structural requirements of a square are met, balancing simplicity and uniqueness checks with validation through distance evaluations.

