# [LeetCode 149: Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/)

## Approaches
- [Brute Force](#brute-force)
- [Optimized Using HashMap for Slopes](#optimized-using-hashmap-for-slopes)

## Brute Force

### Intuition
The brute force approach involves checking every pair of points to determine the line they define. We keep track of how many points lie on each line by calculating the slope between two points. The slope provides a unique identifier for each line. By iterating over each point and comparing it with every other point, we count how many points lie on the same slope, which gives us the maximum number of points on a line.

### Approach
1. Iterate over all pairs of points.
2. For each pair, calculate the slope.
3. Use a map (dictionary) to store the count of points having the same slope.
4. Keep track of the maximum count for each point considered as a starting point.
5. Return the maximum of all maximum counts found.

### Time Complexity
- O(n^2), where n is the number of points as we check every pair of points.

### Space Complexity
- O(n), due to the storage required for storing slopes in the map.

```go
func maxPoints(points [][]int) int {
    if len(points) <= 2 {
        return len(points)
    }

    var maxPoints int
    for i := 0; i < len(points); i++ {
        slopes := make(map[string]int)
        duplicate := 1
        currentMax := 0

        for j := 0; j < len(points); j++ {
            if i == j {
                continue
            }

            dx := points[j][0] - points[i][0]
            dy := points[j][1] - points[i][1]

            if dx == 0 && dy == 0 {
                duplicate++
                continue
            }

            gcd := findGCD(dx, dy)

            dx /= gcd
            dy /= gcd

            slope := fmt.Sprintf("%d/%d", dy, dx)
            slopes[slope]++
            currentMax = max(currentMax, slopes[slope])
        }

        maxPoints = max(maxPoints, currentMax + duplicate)
    }

    return maxPoints
}

func findGCD(a, b int) int {
    if b == 0 {
        return a
    }
    return findGCD(b, a % b)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Optimized Using HashMap for Slopes

### Intuition
This approach optimizes the brute force slightly by using hash maps to efficiently calculate and store slopes. Instead of computationally expensive comparisons, we leverage a hash map to count occurrences of unique slopes. This method reduces the necessity of managing duplicate calculations within nested loops by focusing only on necessary slope comparisons.

### Approach
1. For each point, compute the slope with all other points.
2. Normalize the slope by using a Greatest Common Divisor (GCD) to handle floating-point precision and to make comparison simpler.
3. Use a hash map to store and count unique slope representations.
4. Consider the case of vertical and overlapping lines separately.
5. Track maximum points for each starting point including overlapping and update the global maximum.

### Time Complexity
- O(n^2), because you need to iterate through each point pairwise.

### Space Complexity
- O(n), for storing slopes for each point's comparison.

```go
func maxPoints(points [][]int) int {
    n := len(points)
    if n <= 2 {
        return n
    }

    var result int

    for i := 0; i < n; i++ {
        duplicates := 1
        verticals := 0
        slopes := make(map[string]int)
        curMax := 0

        for j := 0; j < n; j++ {
            if i == j {
                continue
            }

            dx := points[j][0] - points[i][0]
            dy := points[j][1] - points[i][1]

            if dx == 0 && dy == 0 {
                duplicates++
                continue
            }
            
            if dx == 0 {
                verticals++
                continue
            }

            gcd := findGCD(dx, dy)
            dx /= gcd
            dy /= gcd

            slope := fmt.Sprintf("%d/%d", dy, dx)
            slopes[slope]++
            curMax = max(curMax, slopes[slope])
        }

        curMax = max(curMax, verticals)
        result = max(result, curMax + duplicates)
    }

    return result
}
```

Both approaches fundamentally calculate the same result, with the latter approach offering slightly simplified handling of various line orientations using algebraic properties.

