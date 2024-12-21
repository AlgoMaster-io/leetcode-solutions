# [Leetcode 593: Valid Square](https://leetcode.com/problems/valid-square/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Mathematical Approach Using Distance](#mathematical-approach-using-distance)

### Brute Force Approach

#### Intuition:
To determine if four given points form a square, we can list all permutations of the points and validate if any permutation forms a valid square. A valid square should have four equal sides and two equal diagonals.

#### Approach:
- Check all permutations of the given four points treating them as vertices `p1, p2, p3, p4`.
- Ensure the condition of a square is satisfied, i.e., if distances between adjacent vertices are equal and the diagonals also have equal length.
- Calculate the distance between every pair to confirm they meet the above conditions.

#### Time and Space Complexity:
- **Time Complexity**: \(O(1)\), permutations of four points is constant.
- **Space Complexity**: \(O(1)\), as we only use fixed extra space for computations.

```python
import itertools

def distance(p1, p2):
    return (p2[0] - p1[0])**2 + (p2[1] - p1[1])**2

def is_square(p1, p2, p3, p4):
    d1 = distance(p1, p2)
    d2 = distance(p2, p3)
    d3 = distance(p3, p4)
    d4 = distance(p4, p1)
    d5 = distance(p1, p3)
    d6 = distance(p2, p4)
    return d1 == d2 == d3 == d4 and d5 == d6 and d1 > 0

def validSquare(p1, p2, p3, p4):
    points = [p1, p2, p3, p4]
    for perm in itertools.permutations(points):
        if is_square(*perm):
            return True
    return False
```

### Mathematical Approach Using Distance

#### Intuition:
Without permutations, we can determine the square validity using the distances between all pairs of points. For a valid square, we need exactly two distinct distances: one being the side of the square and the other being the diagonal.

#### Approach:
- Calculate the distance between all pairs of points.
- Check if there are only two different lengths: one repeats four times (square's side) and the other repeats twice (square's diagonal).
- Ensure the side length is non-zero to avoid degenerate cases.

#### Time and Space Complexity:
- **Time Complexity**: \(O(1)\), as it involves calculating distance for constant pairs.
- **Space Complexity**: \(O(1)\), only constant extra space.

```python
def validSquare(p1, p2, p3, p4):
    def dist_sq(a, b):
        return (a[0] - b[0])**2 + (a[1] - b[1])**2
    
    dists = [
        dist_sq(p1, p2),
        dist_sq(p1, p3),
        dist_sq(p1, p4),
        dist_sq(p2, p3),
        dist_sq(p2, p4),
        dist_sq(p3, p4)
    ]
    
    # Count occurrences of each distance
    count = {}
    for d in dists:
        if d in count:
            count[d] += 1
        else:
            count[d] = 1
    
    # Valid square has two distinct distances: one 4 times (sides), one 2 times (diagonals)
    if len(count) != 2:
        return False
    
    # Check the occurrences of distances
    side, diagonal = sorted(count.items(), key=lambda x: x[1])
    return side[1] == 4 and diagonal[1] == 2 and side[0] > 0
```

Each approach above ensures we accurately check the conditions necessary for four points to form a valid square. Each method uses different principles, providing a balance of simplicity and mathematical reliability.

