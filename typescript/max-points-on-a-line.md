### [Leetcode 149: Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/)

### Solutions:

1. [Brute Force - Calculate Slope for Every Pair](#solution-1)
2. [Optimized Using HashMap for Slopes](#solution-2)

---

### Solution 1: Brute Force - Calculate Slope for Every Pair

**Intuition:**

The basic idea is to check every pair of points, determine the slope, and check how many other points are on the line defined by that slope. This approach is straightforward but can result in a lot of repeated work.

- **Steps:**
  - For each point, calculate the slope with every other point.
  - Count the number of points sharing that same slope.

```typescript
function maxPoints(points: number[][]): number {
    if (points.length < 3) return points.length;

    // Function to compute the slope given two points
    const getSlope = (p1: number[], p2: number[]): string => {
        const dx = p2[0] - p1[0];
        const dy = p2[1] - p1[1];
        if (dx === 0) return 'vertical';   // Special case: vertical slope
        if (dy === 0) return 'horizontal'; // Special case: horizontal slope
        const gcd = (a: number, b: number): number => b === 0 ? a : gcd(b, a % b);
        const gcdValue = gcd(dx, dy);
        return `${dy / gcdValue}/${dx / gcdValue}`; // Represent slope as a simplified fraction
    };

    let maxPointsOnLine = 1;

    // Consider each point as the base
    for (let i = 0; i < points.length; i++) {
        const slopes = new Map<string, number>();
        
        for (let j = i + 1; j < points.length; j++) {
            const slope = getSlope(points[i], points[j]);
            slopes.set(slope, (slopes.get(slope) || 0) + 1);
        }
        
        // Find the maximum number of points sharing the same slope
        for (const count of slopes.values()) {
            maxPointsOnLine = Math.max(maxPointsOnLine, count + 1); // +1 to include the base point
        }
    }
    
    return maxPointsOnLine;
}
```

- **Time Complexity:** \(O(n^2 \cdot \log(n))\) where \( \log(n) \) is for reducing slope fractions using GCD.
- **Space Complexity:** \(O(n)\) for the hashmap storing slope counts.

---

### Solution 2: Optimized Using HashMap for Slopes

**Intuition:**

To optimize further:
1. Use a hash map to store the count of points sharing the same slope from a given point.
2. Track and ignore points that are coincident.

- **Steps:**
  - Use a map to count the lines through a single point.
  - Add a special case for vertical lines and overlapping points.

```typescript
function maxPointsOptimized(points: number[][]): number {
    if (points.length <= 2) return points.length;

    let result = 0;

    for (let i = 0; i < points.length; i++) {
        let duplicate = 1; // Count the point itself
        const slopes = new Map<string, number>();

        for (let j = 0; j < points.length; j++) {
            if (i == j) continue;

            const dx = points[j][0] - points[i][0];
            const dy = points[j][1] - points[i][1];

            if (dx === 0 && dy === 0) {
                duplicate++;
                continue;
            }

            const gcdValue = gcd(dx, dy);
            const slope = `${dy / gcdValue}/${dx / gcdValue}`;
            slopes.set(slope, (slopes.get(slope) || 0) + 1);
        }

        const currentMax = Array.from(slopes.values()).reduce((max, count) => Math.max(max, count), 0);
        result = Math.max(result, currentMax + duplicate);
    }
    return result;
}

function gcd(a: number, b: number): number {
    return b === 0 ? a : gcd(b, a % b);
}
```

- **Time Complexity:** \(O(n^2 \cdot \log(n))\), but with reduced repetitive calculations due to optimized slope handling.
- **Space Complexity:** \(O(n)\) for the hashmap storing slope counts.

