# [149. Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/)

## Table of Contents
- [Brute Force Solution](#brute-force-solution)
- [Optimized Solution using HashMap](#optimized-solution-using-hashmap)

---

## Brute Force Solution

### Intuition
The brute force solution involves checking all combinations of two points to formulate a line equation and count how many points lie on the formulated line. This involves calculating the slope for each pair of points and checking if other points have the same slope with the base point.

### Approach
1. Loop through each point and treat it as a starting point.
2. For each starting point, loop through every other point to calculate the slope between them.
3. Check if other points have the same slope when compared to the starting point.
4. Keep track of the maximum count of collinear points found for any starting point.

### Code
```javascript
function maxPoints(points) {
    if (points.length <= 2) return points.length;
    
    let maxPointsOnLine = 0;

    for (let i = 0; i < points.length; i++) {
        let pointCount = new Map();
        let duplicates = 0;
        let maxPointsForCurrent = 0;

        for (let j = i + 1; j < points.length; j++) {
            let dx = points[j][0] - points[i][0];
            let dy = points[j][1] - points[i][1];

            if (dx === 0 && dy === 0) {
                duplicates++;
                continue;
            }

            const gcdValue = gcd(dx, dy);
            dx /= gcdValue;
            dy /= gcdValue;

            const key = `${dx}:${dy}`;
            pointCount.set(key, (pointCount.get(key) || 0) + 1);

            maxPointsForCurrent = Math.max(maxPointsForCurrent, pointCount.get(key));
        }

        maxPointsOnLine = Math.max(maxPointsOnLine, maxPointsForCurrent + duplicates + 1);
    }

    return maxPointsOnLine;
}

function gcd(a, b) {
    if (b === 0) return a;
    return gcd(b, a % b);
}
```

### Time Complexity
- **O(N^3)**, where N is the number of points. We go through each pair of points, and for each pair, potentially iterate through all points.

### Space Complexity
- **O(N)**, using a map to count slopes for a base point.

---

## Optimized Solution using HashMap

### Intuition
The optimized approach leverages a hashmap to store slopes between a point and every other point. The slope is a crucial factor because it defines whether two points are collinear. Storing slopes effectively allows us to count how many points share the same slope with a given starting point efficiently.

### Approach
1. For each point, use a hashmap to store the slope between it and every other point.
2. Use a helper function to calculate the greatest common divisor (GCD) to store the slope in its simplest form.
3. Track duplicates separately since they are always collinear with the base point.
4. Record the maximum count of collinear points for each point.

### Code

```javascript
function maxPoints(points) {
    if (points.length <= 2) return points.length;

    let maxPointsOnLine = 0;

    for (let i = 0; i < points.length; i++) {
        let slopeMap = new Map();
        let duplicates = 0;
        let maxPointsForCurrent = 0;

        for (let j = i + 1; j < points.length; j++) {
            let dx = points[j][0] - points[i][0];
            let dy = points[j][1] - points[i][1];

            if (dx === 0 && dy === 0) {
                duplicates++;
                continue;
            }

            const gcdValue = gcd(dx, dy);
            dx /= gcdValue;
            dy /= gcdValue;

            const key = `${dx}:${dy}`;
            slopeMap.set(key, (slopeMap.get(key) || 0) + 1);

            maxPointsForCurrent = Math.max(maxPointsForCurrent, slopeMap.get(key));
        }

        maxPointsOnLine = Math.max(maxPointsOnLine, maxPointsForCurrent + duplicates + 1);
    }

    return maxPointsOnLine;
}

function gcd(a, b) {
    if (b === 0) return a;
    return gcd(b, a % b);
}
```

### Time Complexity
- **O(N^2)**, for each pair of points, we compute the slope.

### Space Complexity
- **O(N)**, needed for the hashmap to store slopes for each base point.

