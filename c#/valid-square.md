# [Leetcode 593: Valid Square](https://leetcode.com/problems/valid-square/)

## Approaches
1. [Basic Approach: Check all pairwise distances](#check-distances)
2. [Math-Based Optimized Approach: Check properties of a square](#properties-of-square)

---

### 1. Basic Approach: Check all pairwise distances

**Intuition:**

A square has the following properties:
- All sides are of equal length.
- The diagonals are equal and longer than the sides.

We can compute the pairwise distances between the given four points and ensure there are only two distinct distances (sides and diagonals), with the diagonal distance being the largest.

**Approach:**

1. Use a helper function to compute the square of the Euclidean distance between two points.
2. Compute and store all six pairwise distances.
3. Use a HashSet to record distinct distances.
4. Verify that there are only two distinct distances (one side length and one diagonal).
5. Count the occurrences and verify:
   - The smaller distance occurs 4 times (sides).
   - The larger distance occurs 2 times (diagonals).

```csharp
public class Solution {
    public bool ValidSquare(int[] p1, int[] p2, int[] p3, int[] p4) {
        // Helper function to calculate squared Euclidean distance
        int DistSq(int[] p1, int[] p2) {
            return (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1]);
        }

        // Calculate the squared distances between each pair of points
        var distances = new int[] {
            DistSq(p1, p2), DistSq(p1, p3), DistSq(p1, p4),
            DistSq(p2, p3), DistSq(p2, p4),
            DistSq(p3, p4)
        };

        // Use a dictionary to count occurrences of each distinct distance
        var distCount = new Dictionary<int, int>();
        
        foreach (var dist in distances) {
            if (dist == 0) return false; // If any distance is zero, two points overlap, not forming a valid square.
            if (distCount.ContainsKey(dist)) {
                distCount[dist]++;
            } else {
                distCount[dist] = 1;
            }
        }

        // There must be exactly two different distances: 4 equal sides, 2 equal diagonals
        if (distCount.Count != 2) return false;

        // Validate the frequencies of distances
        var values = distCount.Values;
        return values.Contains(4) && values.Contains(2);
    }
}
```

**Time Complexity:** \(O(1)\)

**Space Complexity:** \(O(1)\)

---

### 2. Math-Based Optimized Approach: Check properties of a square

**Intuition:**

Another approach is to focus on ensuring exactly four equal sides and two equal diagonals by analyzing the properties of vectors.

**Approach:**

1. Calculate vectors for the points.
2. Calculate the dot products to ensure perpendicularity.
3. Ensure opposite sides and diagonals are equal in length.

This method reuses the concept of checking distances but focuses on vector properties.

```csharp
public class Solution {
    public bool ValidSquare(int[] p1, int[] p2, int[] p3, int[] p4) {
        int DistanceSquared(int[] a, int[] b) {
            return (a[0] - b[0]) * (a[0] - b[0]) + (a[1] - b[1]) * (a[1] - b[1]);
        }

        int[][] points = new int[][] { p1, p2, p3, p4 };

        Array.Sort(points, (a, b) => a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);

        int d1 = DistanceSquared(points[0], points[1]);
        int d2 = DistanceSquared(points[1], points[3]);
        int d3 = DistanceSquared(points[3], points[2]);
        int d4 = DistanceSquared(points[2], points[0]);
        int diag1 = DistanceSquared(points[0], points[3]);
        int diag2 = DistanceSquared(points[1], points[2]);

        return d1 > 0 && d1 == d2 && d2 == d3 && d3 == d4 && diag1 == diag2;
    }
}
```

**Time Complexity:** \(O(1)\)

**Space Complexity:** \(O(1)\)

---

