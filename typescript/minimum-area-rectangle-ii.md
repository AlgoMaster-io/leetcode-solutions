# [Leetcode 963: Minimum Area Rectangle II](https://leetcode.com/problems/minimum-area-rectangle-ii/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Hashing with Geometry Approach (Optimal)](#hashing-with-geometry-approach-optimal)

## Brute Force Approach

### Intuition
The straightforward way to solve this problem is to iterate over all possible sets of four points and determine if they form a rectangle. This can be achieved by checking:
- The diagonals of the rectangle (which are equal in length and intersect at the rectangle's center).
- Both the pairs of opposite sides are equal in length.
Then, calculate the area for each rectangle and track the minimum.

### Time Complexity
- **Time**: O(N^4) - We check every combination of 4 points.
- **Space**: O(1) - We use a constant amount of extra space.

### TypeScript Code

```typescript
function minAreaFreeRect(points: number[][]): number {
    const n = points.length;
    let minArea = Infinity;

    // Iterate over all sets of four points
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            for (let k = j + 1; k < n; k++) {
                for (let l = k + 1; l < n; l++) {
                    // Extract current four points
                    const [p1, p2, p3, p4] = [points[i], points[j], points[k], points[l]];

                    // Utility function to calculate squared distance
                    const distSq = (a: number[], b: number[]) =>
                        (a[0] - b[0]) ** 2 + (a[1] - b[1]) ** 2;

                    // Calculate all pairs of distances
                    const dists = [
                        distSq(p1, p2), distSq(p1, p3), distSq(p1, p4),
                        distSq(p2, p3), distSq(p2, p4), distSq(p3, p4)
                    ].sort((a, b) => a - b);

                    // Check diagonal and side conditions for rectangle
                    if (dists[0] > 0 &&
                        dists[0] === dists[1] &&
                        dists[2] === dists[3] &&
                        dists[4] === dists[5] &&
                        dists[0] + dists[1] === dists[5]) {

                        // Compute area using geometry (determinant method)
                        const area = Math.sqrt(dists[0] * dists[2]);
                        minArea = Math.min(minArea, area);
                    }
                }
            }
        }
    }

    // If no rectangle has been found, return 0
    return minArea < Infinity ? minArea : 0;
}
```

## Hashing with Geometry Approach (Optimal)

### Intuition
Instead of brute forcing, we utilize properties of vectors and hashing:
1. We consider each pair of points and treat them as possible diagonals.
2. Calculate the center of the diagonal since for a rectangle, the diagonals share the same center.
3. Use a hash map to store potential diagonals sharing the same center.
4. If two diagonals have the same center and their sides are equal, a rectangle is formed.

### Time Complexity
- **Time**: O(N^2) - We check every pair of points and use hashing to track potential diagonals.
- **Space**: O(N^2) - Map space for storing pairs of points (potential diagonals).

### TypeScript Code

```typescript
function minAreaFreeRect(points: number[][]): number {
    const n = points.length;
    let minArea = Infinity;
    const centerMap: Map<string, number[][]> = new Map();

    // Iterate over all pairs of points
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            // Calculate the center point of the diagonal
            const [x1, y1] = points[i];
            const [x2, y2] = points[j];
            const cx = (x1 + x2) / 2;
            const cy = (y1 + y2) / 2;

            // Create a unique key for the center
            const centerKey = `${cx},${cy}`;

            // Calculate squared distance (length of diagonal)
            const dist = (x1 - x2) ** 2 + (y1 - y2) ** 2;

            // If this center exists, check for possible rectangle
            if (centerMap.has(centerKey)) {
                const pointPairs = centerMap.get(centerKey)!;
                for (const [px, py, d] of pointPairs) {
                    if (d === dist) {
                        // Calculate area and update minimal area
                        const area = Math.sqrt((x1 - px) ** 2 + (y1 - py) ** 2) *
                                     Math.sqrt((x2 - px) ** 2 + (y2 - py) ** 2);
                        minArea = Math.min(minArea, area);
                    }
                }
            }

            // Add the current pair to the center map
            if (!centerMap.has(centerKey)) {
                centerMap.set(centerKey, []);
            }
            centerMap.get(centerKey)!.push([x1, y1, dist]);
        }
    }

    // If no rectangle has been found, return 0
    return minArea < Infinity ? minArea : 0;
}
```

This approach leverages vector mathematics and efficient hashing for a more optimal solution.

