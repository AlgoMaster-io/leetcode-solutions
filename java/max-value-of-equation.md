# [1499. Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Approach using Priority Queue](#optimized-approach-using-priority-queue)
3. [Optimal Approach using Deque](#optimal-approach-using-deque)

---

### Brute Force Approach

The brute force approach involves iterating through each pair of points and calculating the possible equation values directly. This results in a time-consuming solution, but it's a good starting point to understand the problem.

#### Intuition

1. Iterate over every possible pair of points `(i, j)` where `i < j`.
2. Check if the condition `|xi - xj| <= k` holds.
3. If it holds, compute the equation value `(yi + yj + |xi - xj|)`.
4. Track the maximum equation value found.

#### Code

```java
class Solution {
    public int findMaxValueOfEquation(int[][] points, int k) {
        int maxValue = Integer.MIN_VALUE;
        for (int i = 0; i < points.length; i++) {
            for (int j = i + 1; j < points.length; j++) {
                int x1 = points[i][0];
                int y1 = points[i][1];
                int x2 = points[j][0];
                int y2 = points[j][1];
                // Check if the condition is satisfied
                if (x2 - x1 <= k) {
                    // Calculate the equation value
                    maxValue = Math.max(maxValue, y1 + y2 + x2 - x1);
                }
            }
        }
        return maxValue;
    }
}
```

#### Time Complexity

- **Time:** O(n^2), where n is the number of points, as we are checking every pair.
- **Space:** O(1), no extra space is used apart from variables.

---

### Optimized Approach using Priority Queue

To improve on the brute force approach, we can use a priority queue to help us find pairs that maximize the expression efficiently.

#### Intuition

1. Use a max heap (priority queue) to maintain the maximum values of `yi - xi` pairs.
2. Iterate over points while maintaining only valid points in the heap (those satisfying `xj - xi <= k`).
3. For each point, calculate the potential value using the max in the priority queue.
4. Update the priority queue with the current point.

#### Code

```java
import java.util.PriorityQueue;

class Solution {
    public int findMaxValueOfEquation(int[][] points, int k) {
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> b[0] - a[0]);
        int maxValue = Integer.MIN_VALUE;

        for (int[] point : points) {
            int xj = point[0];
            int yj = point[1];

            // Remove elements from the heap where the condition is not met
            while (!maxHeap.isEmpty() && xj - maxHeap.peek()[1] > k) {
                maxHeap.poll();
            }

            // If the heap is not empty, calculate the equation value
            if (!maxHeap.isEmpty()) {
                maxValue = Math.max(maxValue, yj + xj + maxHeap.peek()[0]);
            }

            // Add the current point (yj - xj) to the heap
            maxHeap.offer(new int[]{yj - xj, xj});
        }
        return maxValue;
    }
}
```

#### Time Complexity

- **Time:** O(n log n), because each point is inserted and removed from the heap.
- **Space:** O(n), due to the space needed for the heap.

---

### Optimal Approach using Deque

To achieve even better time efficiency, we can use a deque data structure that allows insertion and deletions at both ends efficiently.

#### Intuition

1. Use a deque to maintain indices of elements such that the potential maximum calculation `(yi - xi)` remains at the front.
2. For each point, first, clean the deque of elements that do not satisfy `|xi - xj| <= k`.
3. Calculate maximum potential value using the front of the deque.
4. Maintain the deque such that it stores only necessary indices for upcoming iterations.

#### Code

```java
import java.util.Deque;
import java.util.LinkedList;

class Solution {
    public int findMaxValueOfEquation(int[][] points, int k) {
        Deque<int[]> deque = new LinkedList<>();
        int maxValue = Integer.MIN_VALUE;

        for (int[] point : points) {
            int xj = point[0];
            int yj = point[1];

            // Remove outdated points from the deque
            while (!deque.isEmpty() && xj - deque.peekFirst()[1] > k) {
                deque.pollFirst();
            }

            // Calculate potential max with the deque's first element
            if (!deque.isEmpty()) {
                maxValue = Math.max(maxValue, yj + xj + deque.peekFirst()[0]);
            }

            // Maintain deque in a descending order of yi - xi
            while (!deque.isEmpty() && yj - xj >= deque.peekLast()[0]) {
                deque.pollLast();
            }

            // Add current point to the deque
            deque.offerLast(new int[]{yj - xj, xj});
        }

        return maxValue;
    }
}
```

#### Time Complexity

- **Time:** O(n), since each element is inserted and removed from the deque at most once.
- **Space:** O(n), space required for the deque.

