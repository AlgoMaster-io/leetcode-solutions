# Leetcode Problem: [1499. Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Two Pointer Approach Using SortedList](#approach-2-two-pointer-approach-using-sortedlist)
- [Approach 3: Monotonic Deque (Optimal)](#approach-3-monotonic-deque-optimal)

---

## Approach 1: Brute Force

### Intuition:
The brute force approach tries to compute the desired equation value for all pairs of points within the allowed distance `k`. For each point `i`, search for all points `j` where `j > i` and `|xi - xj| <= k`. This approach is straightforward but computationally expensive.

### Steps:
1. Iterate through each point as `i`.
2. For each point `i`, iterate over all subsequent points `j`.
3. Check if the distance condition `|xi - xj| <= k` holds.
4. Calculate the equation value for valid pairs and keep track of the maximum.

### Time Complexity:
- **O(n^2):** We are iterating through all pairs of points.

### Space Complexity:
- **O(1):** No extra space other than variables for calculations.

### C# Code:

```csharp
public class Solution {
    public int FindMaxValueOfEquation(int[][] points, int k) {
        int maxEquationValue = int.MinValue;
        int n = points.Length;
        
        // Iterate over all pairs (i, j)
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                // Check if the condition |xi - xj| <= k is met
                if (Math.Abs(points[i][0] - points[j][0]) <= k) {
                    // Calculate equation value
                    int equationValue = points[i][1] + points[j][1] + Math.Abs(points[i][0] - points[j][0]);
                    maxEquationValue = Math.Max(maxEquationValue, equationValue);
                }
            }
        }
        
        return maxEquationValue;
    }
}
```

---

## Approach 2: Two Pointer Approach Using SortedList

### Intuition:
By using a sorted data structure like `SortedList`, we can improve performance. Instead of checking all pairs, we can efficiently find the point `j` that maximizes the equation value for a given point `i` within the allowed distance `k` by maintaining a sorted order of previously encountered values.

### Steps:
1. Utilize a `SortedList` to store `(y - x, x)` for points within valid range.
2. As we iterate through the list, for each point `i`, search for a point `j` where `i < j` and `xj - xi <= k`.
3. Calculate the max value and update the SortedList accordingly.

### Time Complexity:
- **O(n log n):** Each insertion and deletion into the `SortedList` requires logarithmic time.

### Space Complexity:
- **O(n):** We store up to `n` entries in the sorted list.

### C# Code:

```csharp
using System.Collections.Generic;

public class Solution {
    public int FindMaxValueOfEquation(int[][] points, int k) {
        int maxEquationValue = int.MinValue;
        var sortedList = new SortedList<int, int>();
        
        foreach (var point in points) {
            int x = point[0], y = point[1];
            
            // Remove elements from sortedList if their x-values are out of bounds
            while (sortedList.Count > 0 && sortedList.Keys[0] < x - k) {
                sortedList.RemoveAt(0);
            }
            
            // If sortedList is not empty, calculate max value for the current point
            if (sortedList.Count > 0) {
                int maxYMinusX = sortedList.First().Key;
                maxEquationValue = Math.Max(maxEquationValue, maxYMinusX + y + x);
            }
            
            // Store current point (y - x, x) in the sorted order
            int currentYMinusX = y - x;
            sortedList.Add(currentYMinusX, x);
        }
        
        return maxEquationValue;
    }
}
```

---

## Approach 3: Monotonic Deque (Optimal)

### Intuition:
With the monotonic deque, we only keep the potentially useful points `(xj, yj)` for maximizing our result relative to a given point `(xi, yi)`. By storing elements in a deque, we can efficiently find and remove elements that are too far from the current point or cannot contribute to a better result.

### Steps:
1. Use a deque to store elements `(yj - xj, xj)`.
2. For each point `(xi, yi)`, perform the following:
   - Remove points from the deque that are farther than `k` from `xi`.
   - Calculate the potential max value from the front of the deque.
   - Maintain a decreasing order by removing elements that are dominated by the current one.
   - Add the new point `(yi - xi, xi)` to the deque.

### Time Complexity:
- **O(n):** Each element is added and removed from the deque at most once.

### Space Complexity:
- **O(n):** We maintain a deque of elements up to the length of `points`.

### C# Code:

```csharp
using System.Collections.Generic;

public class Solution {
    public int FindMaxValueOfEquation(int[][] points, int k) {
        int maxEquationValue = int.MinValue;
        var deque = new LinkedList<(int value, int x)>();
        
        foreach (var point in points) {
            int x = point[0], y = point[1];
            
            // Remove elements that are out of the range xj <= xi + k
            while (deque.Count > 0 && deque.First.Value.x < x - k) {
                deque.RemoveFirst();
            }
            
            // If deque has valid elements, calculate the max value
            if (deque.Count > 0) {
                maxEquationValue = Math.Max(maxEquationValue, deque.First.Value.value + x + y);
            }
            
            int newValue = y - x;
            // Maintain the monotonicity of the deque
            while (deque.Count > 0 && deque.Last.Value.value <= newValue) {
                deque.RemoveLast();
            }
            
            deque.AddLast((newValue, x));
        }
        
        return maxEquationValue;
    }
}
```

This solution efficiently computes the max value of the equation using a monotonic deque, making it the optimal approach in terms of time complexity.

