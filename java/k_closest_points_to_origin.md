# 973. [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approach 1: Max-Heap

### Solution
```java
// Time Complexity: O(n * log(k)), where n is the number of points
// Space Complexity: O(k), for the max-heap
import java.util.PriorityQueue;

public class Solution {
    public int[][] kClosest(int[][] points, int k) {
        // Max-Heap to store the k closest points
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> 
            Integer.compare(distance(b), distance(a))
        );

        // Add points to the heap and maintain only the k closest points
        for (int[] point : points) {
            maxHeap.add(point);
            if (maxHeap.size() > k) {
                maxHeap.poll(); // Remove the farthest point
            }
        }

        // Extract the k closest points from the heap
        int[][] result = new int[k][2];
        for (int i = 0; i < k; i++) {
            result[i] = maxHeap.poll();
        }

        return result;
    }

    // Helper method to calculate the squared distance from the origin
    private int distance(int[] point) {
        return point[0] * point[0] + point[1] * point[1];
    }
}
```

## Approach 2: Quickselect

### Solution
```java
// Time Complexity: O(n) on average, O(n^2) in the worst case
// Space Complexity: O(1), as it modifies the input array in-place
import java.util.Random;

public class Solution {
    public int[][] kClosest(int[][] points, int k) {
        quickSelect(points, 0, points.length - 1, k);
        int[][] result = new int[k][2];
        System.arraycopy(points, 0, result, 0, k);
        return result;
    }

    private void quickSelect(int[][] points, int left, int right, int k) {
        if (left >= right) return;

        int pivotIndex = partition(points, left, right);
        if (pivotIndex == k) {
            return;
        } else if (pivotIndex < k) {
            quickSelect(points, pivotIndex + 1, right, k);
        } else {
            quickSelect(points, left, pivotIndex - 1, k);
        }
    }

    private int partition(int[][] points, int left, int right) {
        int[] pivot = points[right];
        int pivotDistance = distance(pivot);
        int i = left;

        for (int j = left; j < right; j++) {
            if (distance(points[j]) < pivotDistance) {
                swap(points, i, j);
                i++;
            }
        }
        swap(points, i, right);
        return i;
    }

    private void swap(int[][] points, int i, int j) {
        int[] temp = points[i];
        points[i] = points[j];
        points[j] = temp;
    }

    private int distance(int[] point) {
        return point[0] * point[0] + point[1] * point[1];
    }
}
```