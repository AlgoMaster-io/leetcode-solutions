# [Leetcode 593: Valid Square](https://leetcode.com/problems/valid-square/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1)
- [Approach 2: Using Distances with Mathematical Properties](#approach-2)

## Approach 1: Brute Force

### Intuition
To determine if four points can form a square, each point must be connected by edges of equal length, and the diagonals connecting opposite corners should also be equal in length but longer than the edges. A brute force solution would involve calculating the distance between every pair of points and checking if they fit these criteria.

### Solution
In this approach, we calculate the distances between all pairs of points. We expect four equal lengths representing the sides of the square and two longer equal lengths representing the diagonals.

```typescript
function distanceSquared(p1: number[], p2: number[]): number {
    return (p1[0] - p2[0]) ** 2 + (p1[1] - p2[1]) ** 2;
}

function validSquare(p1: number[], p2: number[], p3: number[], p4: number[]): boolean {
    const points = [p1, p2, p3, p4];
    const distances: number[] = [];

    // Calculate distance between each pair of points
    for (let i = 0; i < points.length; ++i) {
        for (let j = i + 1; j < points.length; ++j) {
            distances.push(distanceSquared(points[i], points[j]));
        }
    }

    // Sort distances to easily check equal sides and diagonals
    distances.sort((a, b) => a - b);

    // A valid square should have four equal smallest distances (sides)
    // and two equal largest distances (diagonals)
    return (
        distances[0] > 0 &&
        distances[0] === distances[1] &&
        distances[1] === distances[2] &&
        distances[2] === distances[3] &&
        distances[4] === distances[5] &&
        distances[3] < distances[4]
    );
}
```

### Time Complexity
- **O(1)**: There are only 6 distances to calculate and compare, making it a constant time solution.

### Space Complexity
- **O(1)**: The space used does not scale with input size.

## Approach 2: Using Distances with Mathematical Properties

### Intuition
To form a square, all sides must be equal and diagonals must be equal. By calculating squared distances and checking these conditions, we can efficiently determine whether the points form a valid square.

### Solution
Instead of comparing all possible distances, we can exploit properties of vector addition to check perpendicularity and equality conditions more directly.

```typescript
function validSquareMathematical(p1: number[], p2: number[], p3: number[], p4: number[]): boolean {
    function isPerpendicular(pa: number[], pb: number[], pc: number[]): boolean {
        // vector ab · vector ac = 0 if they are perpendicular
        return (pb[0] - pa[0]) * (pc[0] - pa[0]) + (pb[1] - pa[1]) * (pc[1] - pa[1]) === 0;
    }
    
    // Calculate squared distances to avoid float precision issues
    const d1 = distanceSquared(p1, p2);
    const d2 = distanceSquared(p1, p3);
    const d3 = distanceSquared(p1, p4);
    const d4 = distanceSquared(p2, p3);
    const d5 = distanceSquared(p2, p4);
    const d6 = distanceSquared(p3, p4);
    
    // Check if any set of two distances form perpendicular (right) angles
    if (!d1 || !d2 || !d3 || !d4 || !d5 || !d6) return false;
    return (
        (d1 === d2 && isPerpendicular(p1, p2, p3) && d3 === d6 && d1 === d3) || 
        (d1 === d3 && isPerpendicular(p1, p2, p4) && d2 === d5 && d1 === d2) || 
        (d2 === d3 && isPerpendicular(p1, p3, p4) && d1 === d4 && d2 === d1)
    );
}
```

### Time Complexity
- **O(1)**: Similar to the first approach; a constant number of steps and comparisons.

### Space Complexity
- **O(1)**: No extra space needed beyond a fixed number of variables.

