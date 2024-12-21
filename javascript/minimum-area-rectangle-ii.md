# [Leetcode 963: Minimum Area Rectangle II](https://leetcode.com/problems/minimum-area-rectangle-ii)

## Approaches
1. [Brute Force Approach](#1-brute-force-approach)
2. [Optimized Approach using Geometry](#2-optimized-approach-using-geometry)

---

## 1. Brute Force Approach

### Intuition
The brute force solution to find the minimum area of a rectangle given a set of points is to try every possible combination of three points and check if they can potentially form a rectangle with the inclusion of a fourth point. If any valid rectangle can be formed, calculate its area. This approach involves considerable computations given four points can form numerous combinations, but it's crucial for understanding the problem constraints and behavior.

### Code
```javascript
var minAreaFreeRect = function(points) {
    let minArea = Infinity; // Initialize the minimum area as infinity
    const n = points.length;

    // Iterate over all combinations of three points
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            for (let k = j + 1; k < n; k++) {
                // Calculate the distances between points
                const dx1 = points[j][0] - points[i][0];
                const dy1 = points[j][1] - points[i][1];
                const dx2 = points[k][0] - points[i][0];
                const dy2 = points[k][1] - points[i][1];

                // Check if vectors are perpendicular
                if (dx1 * dx2 + dy1 * dy2 === 0) {
                    // Calculate the potential fourth point
                    const x4 = points[j][0] + dx2;
                    const y4 = points[j][1] + dy2;

                    // Check if the fourth point exists in the set
                    if (pointSet.has(hashPoint([x4, y4]))) {
                        // Calculate area of the rectangle
                        const area = Math.abs(dx1 * dy2 - dy1 * dx2);
                        minArea = Math.min(minArea, area);
                    }
                }
            }
        }
    }

    return minArea === Infinity ? 0 : minArea;
};

// Helper function to format the point as a string
function hashPoint(point) {
    return point.join(',');
}

// To optimize the checking part, pre-compute point hash points
const pointSet = new Set(points.map(hashPoint));
```

### Complexity
- **Time Complexity:** O(n^3), since we need to check each combination of three points which leads to C(n, 3) points.
- **Space Complexity:** O(n), we store each point once for quick lookup in a set.

---

## 2. Optimized Approach using Geometry

### Intuition
Instead of trying all triplet combinations, a more optimal approach involves using the properties of the geometry of a rectangle and vectors. By examining all pairs of points as potential diagonals, a rectangle can be formed if there exists a fourth point equidistant to both points forming the diagonal.

### Code
```javascript
var minAreaFreeRect = function(points) {
    let minArea = Infinity;
    const n = points.length;

    // Use a map to track point distances with their possible forming pairs
    const pointMap = new Map();

    // Iterate over all pairs of points
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            const dx = points[j][0] - points[i][0];
            const dy = points[j][1] - points[i][1];
            const distSq = dx * dx + dy * dy;

            if (!pointMap.has(distSq)) {
                pointMap.set(distSq, []);
            }

            pointMap.get(distSq).push([i, j]);
        }
    }

    // For each distance, try to form rectangles
    for (let pairs of pointMap.values()) {
        for (let [i, j] of pairs) {
            for (let [k, l] of pairs) {
                if (j === k || i === l) continue;

                // Calculate the area if valid points form rectangle
                const x1 = points[i][0], y1 = points[i][1];
                const x2 = points[j][0], y2 = points[j][1];
                const x3 = points[k][0], y3 = points[k][1];
                const x4 = points[l][0], y4 = points[l][1];

                const area = Math.abs((x1 - x3) * (y2 - y4) - (x2 - x4) * (y1 - y3));
                minArea = Math.min(minArea, area);
            }
        }
    }

    return minArea === Infinity ? 0 : minArea;
};
```

### Complexity
- **Time Complexity:** O(n^2), as we only consider pairs of points to exploit vector properties for rectangle formation.
- **Space Complexity:** O(n^2), due to the storage of point pairs in a map.

This optimized solution leverages geometric properties and vector manipulation to identify minimum area rectangles more efficiently than the brute force approach.

