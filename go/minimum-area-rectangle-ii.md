# [Minimum Area Rectangle II](https://leetcode.com/problems/minimum-area-rectangle-ii/)

This problem involves finding the minimum area of a rectangle formed by some points in the plane. The rectangle's sides may not be parallel to the x or y axes.

## Approaches:
- [Approach 1: Brute Force with All Point Pairs + Verify Rectangles](#approach-1)
- [Approach 2: Optimized using Map of Distances](#approach-2)

---

## Approach 1: Brute Force with All Point Pairs + Verify Rectangles

### Intuition
To find the minimum area rectangle, we start by considering all possible pairs of points as diagonally opposite corners of a rectangle. For each such pair, check whether the remaining two points needed to form a rectangle exist and determine the area if they do.

### Steps
1. Iterate over all pairs of points `(p1, p2)`.
2. For each pair, compute the center of the segment connecting them and their distances.
3. Iterate over all other pairs of points `(p3, p4)` to check if they form two other corners of the rectangle satisfying the center and distance conditions.
4. Calculate the area of the rectangle.

### Code
```go
func minAreaFreeRect(points [][]int) float64 {
    n := len(points)
    if n < 4 {
        return 0.0 // Less than 4 points can't form a rectangle
    }
    pointSet := make(map[[2]int]bool)
    for _, point := range points {
        pointSet[[2]int{point[0], point[1]}] = true
    }
    
    minArea := float64(^uint(0) >> 1) // Infinity
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            for k := j + 1; k < n; k++ {
                for l := k + 1; l < n; l++ {
                    if checkRectangle(points[i], points[j], points[k], points[l]) {
                        area := calculateArea(points[i], points[j], points[k], points[l])
                        if area > 0 {
                            minArea = math.Min(minArea, area)
                        }
                    }
                }
            }
        }
    }
    
    if minArea == float64(^uint(0) >> 1) {
        return 0.0
    }
    return minArea
}

func checkRectangle(a, b, c, d []int) bool {
    if dist(a, b)+dist(c, d) == dist(a, c)+dist(b, d) && dist(a, c)+dist(b, d) == dist(a, d)+dist(b, c) {
        return true
    }
    return false
}

func calculateArea(a, b, c, d []int) float64 {
    // Use vector cross product to get area
    ab := []int{b[0] - a[0], b[1] - a[1]}
    ac := []int{c[0] - a[0], c[1] - a[1]}
    area := math.Sqrt(float64(ab[0]*ac[1] - ac[0]*ab[1]))
    return area
}

func dist(p1, p2 []int) int {
    return (p2[0]-p1[0])*(p2[0]-p1[0]) + (p2[1]-p1[1])*(p2[1]-p1[1])
}
```

### Complexity
- **Time Complexity:** \( O(n^4) \), where \( n \) is the number of points. We are iterating over all combinations of four points.
- **Space Complexity:** \( O(n) \) for storing the set of points.

---

## Approach 2: Optimized using Map of Distances

### Intuition
Instead of checking every possible quadruple of points, we can use a more structured approach by maintaining a map of point midpoints and distances as keys. Two segments with the same midpoint and the same distance define diagonals of rectangles.

### Steps
1. Use a hash map to store all midpoints and distances between pairs of points.
2. For each entry, if multiple pairs exist, they form diagonals of rectangles with calculable areas.
3. Calculate the rectangles' areas and track the minimum.

### Code
```go
func minAreaFreeRect(points [][]int) float64 {
    type Midpoint struct {
        x, y float64
    }
    
    midpoints := make(map[Midpoint][][2]int) // Map of midpoint to pair of points indices
    
    n := len(points)
    minArea := math.MaxFloat64
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            // Calculate midpoint and squared distance
            mid := Midpoint{
                x: float64(points[i][0]+points[j][0]) / 2.0,
                y: float64(points[i][1]+points[j][1]) / 2.0,
            }
            distSq := dist(points[i], points[j])
            
            // Check existing midpoints
            if pairs, exists := midpoints[mid]; exists {
                for _, pair := range pairs {
                    a, b := pair[0], pair[1]
                    area := math.Sqrt(float64(dist(points[a], points[i])) * float64(dist(points[b], points[j])))
                    minArea = math.Min(minArea, area)
                }
            }
            
            // Append current pair for midpoint
            midpoints[mid] = append(midpoints[mid], [2]int{i, j})
        }
    }
    if minArea == math.MaxFloat64 {
        return 0.0
    }
    return minArea
}

```

### Complexity
- **Time Complexity:** \( O(n^2) \), we are iterating over all pairs of points and storing them in a map.
- **Space Complexity:** \( O(n^2) \) to maintain the midpoints map.

This solution effectively reduces the problem to a pairwise comparison with a hash map, which is optimal for large inputs.

