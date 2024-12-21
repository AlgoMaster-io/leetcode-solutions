# [Valid Square](https://leetcode.com/problems/valid-square/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Optimized using Distance](#approach-2-optimized-using-distance)

## Approach 1: Brute Force

### Intuition
To determine if four points form a square, the key insight is that:
1. All four sides of the square must be of equal length.
2. The two diagonals must also be of equal length.

Using these properties, we can calculate the distances between all pairs of points and verify the conditions. 

### Solution
```javascript
function distanceSquared(p1, p2) {
    return (p1[0] - p2[0]) ** 2 + (p1[1] - p2[1]) ** 2;
}

function validSquare(p1, p2, p3, p4) {
    const points = [p1, p2, p3, p4];
    const distances = new Set();

    // Calculate distance squared between all pairs of points
    for (let i = 0; i < points.length; i++) {
        for (let j = i + 1; j < points.length; j++) {
            distances.add(distanceSquared(points[i], points[j]));
        }
    }

    // A valid square must have exactly two different distances: the side and the diagonal
    return distances.size === 2 && !distances.has(0);
}

// Example Usage
console.log(validSquare([0,0], [1,1], [1,0], [0,1])); // Output: true
```

### Time Complexity
- **O(1)**: We are only calculating fixed distance pairs (6 pairs), hence constant time.

### Space Complexity
- **O(1)**: We are storing at most 6 values in the set.

## Approach 2: Optimized using Distance

### Intuition
The optimized approach is based on the fact that after calculating the distance between all pairs, a square should have exactly:
- 4 equal shortest distances (representing the four sides)
- 2 equal longer distances (representing the diagonals)

### Solution
```javascript
function distanceSquared(p1, p2) {
    return (p1[0] - p2[0]) ** 2 + (p1[1] - p2[1]) ** 2;
}

function validSquare(p1, p2, p3, p4) {
    const points = [p1, p2, p3, p4];
    const distances = [];
    
    // Calculate and store all the distances between pairs of points
    for (let i = 0; i < points.length; i++) {
        for (let j = i + 1; j < points.length; j++) {
            distances.push(distanceSquared(points[i], points[j]));
        }
    }
    
    distances.sort((a, b) => a - b);
    
    // In sorted order, the first 4 and the last 2 should match the condition of a square.
    return distances[0] > 0 && 
           distances[0] === distances[1] &&
           distances[1] === distances[2] &&
           distances[2] === distances[3] &&
           distances[4] === distances[5];
}

// Example Usage
console.log(validSquare([0,0], [1,1], [1,0], [0,1])); // Output: true
```

### Time Complexity
- **O(1)**: Calculation and sorting of a fixed number of distances.

### Space Complexity
- **O(1)**: Storage for a constant number of distances.

By using the approaches outlined, we can verify whether a given set of four points in a 2D plane forms a square effectively.

