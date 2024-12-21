## [Leetcode 963: Minimum Area Rectangle II](https://leetcode.com/problems/minimum-area-rectangle-ii/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Hashing Approach](#optimized-hashing-approach)

---

### Brute Force Approach

**Intuition:**  
The idea is to iterate over all possible pairs of points as the potential diagonals of a rectangle. If two points form the diagonal of a rectangle, then the remaining two sides must be perpendicular to this diagonal and form 90-degree angles at each corner. Once potential rectangles are identified using diagonals, the area can be calculated. This approach involves checking all combinations, which is computationally expensive.

**Algorithm:**
1. Iterate over all pairs of points and consider them as potential diagonals of a rectangle.
2. Check if any other pairs of points exist such that they could form the sides of a rectangle at right angles to this diagonal.
3. Calculate the area using the distance between diagonal points as diagonal length.
4. Track the minimum area found and return it.

```python
from itertools import combinations
from math import sqrt

def minAreaFreeRect(points):
    n = len(points)
    points = [(complex(*p)) for p in points] # Treat points as complex numbers for simplicity
    min_area = float('inf')

    for p1, p2 in combinations(points, 2): # Iterate over pairs of points
        for p3, p4 in combinations(points, 2): # Check if there are other pairs forming a rectangle
            if p1 != p3 and p1 != p4 and p2 != p3 and p2 != p4 and p1 == p3 + (p2 - p4) * 1j: # Check the perpendicular and distance conditions
                area = abs(p2 - p4) * abs(p2 - p3)
                if area < min_area:
                    min_area = area

    return min_area if min_area < float('inf') else 0 # If a valid rectangle is found, return the minimum area

# Time Complexity: O(n^4) - Checking all pairs as potential diagonals with other points forming rectangle sides.
# Space Complexity: O(1)
```

---

### Optimized Hashing Approach

**Intuition:**  
Instead of checking each combination, use the properties of vector coordinates and Euclidean geometry. Store each midpoint and the distance (squared) between pairs of points in a dictionary. A rectangle can be formed if there are two pairs of points with the same midpoint and equal diagonal (distance squared). This reduces the overhead by leveraging the symmetry properties of rectangles.

**Algorithm:**
1. Convert the points into a complex number space to simplify vector arithmetic.
2. Use a dictionary to store tuples of (midpoint, distance squared) as keys and the coordinates of the point pairs as values.
3. For each pair of points, calculate the midpoint and the squared distance, and look for complementary pairs from the dictionary.
4. Upon a match, compute and update the minimum area using derived side lengths.

```python
from collections import defaultdict
from math import sqrt

def minAreaFreeRect(points):
    points = [(complex(x, y)) for x, y in points]
    seen = defaultdict(list)
    min_area = float('inf')

    for i, p1 in enumerate(points):
        for j in range(i):
            # Calculate midpoint
            midpoint = (p1 + points[j]) / 2
            # Calculate squared distance
            dist = abs(p1 - points[j])**2
            seen[(midpoint, dist)].append((p1, points[j]))

    for key, candidates in seen.items():
        if len(candidates) < 2:
            continue
        for (p1, p2), (p3, p4) in combinations(candidates, 2):
            area = abs((p1 - p3) * (p2 - p3))
            if area < min_area:
                min_area = area

    return min_area if min_area < float('inf') else 0

# Time Complexity: O(n^2) - Due to pairs calculation and checking in a hashmap.
# Space Complexity: O(n^2) - Storing midpoint and distance information for point pairs.
```

These approaches illustrate gradually improved solutions, leveraging geometry properties and hashing to optimize the process of finding and calculating potential rectangle structures in the given point set.

