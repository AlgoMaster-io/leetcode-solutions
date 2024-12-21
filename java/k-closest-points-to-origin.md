# [Leetcode 973: K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approaches
1. [Naive Approach: Sorting](#naive-approach-sorting)
2. [Optimized Approach: Heap/Priority Queue](#optimized-approach-heappriority-queue)
3. [Optimal Approach: Quickselect](#optimal-approach-quickselect)

---

### Naive Approach: Sorting

#### Intuition:
The most straightforward way to solve this problem is to calculate the distance of each point from the origin and then sort the points based on these distances.

#### Steps:
1. Calculate the Euclidean distance for each point from the origin.
2. Store the distances along with their respective points.
3. Sort the points based on the calculated distances.
4. Return the first `K` points from the sorted list.

#### Code:
```java
public class Solution {
    public int[][] kClosest(int[][] points, int K) {
        Arrays.sort(points, (a, b) -> {
            // Compare based on distance from origin
            return (a[0] * a[0] + a[1] * a[1]) - (b[0] * b[0] + b[1] * b[1]);
        });
        
        // Return the first K points
        return Arrays.copyOfRange(points, 0, K);
    }
}
```

#### Complexity:
- **Time Complexity**: O(N log N), where N is the number of points. This is due to the sorting step.
- **Space Complexity**: O(1), as we are sorting the array in place.

---

### Optimized Approach: Heap/Priority Queue

#### Intuition:
Instead of sorting the entire array, we can maintain a max heap of size `K` to keep track of the `K` closest points. This allows for more efficient insertion and removal operations as we process each point.

#### Steps:
1. Create a max heap (priority queue) that will store the points based on their distance from the origin.
2. Iterate through each point and calculate its distance from the origin.
3. Maintain the heap to only contain the `K` closest points by removing the farthest point when the heap size exceeds `K`.
4. Convert the result from the heap to an array and return it.

#### Code:
```java
import java.util.PriorityQueue;

public class Solution {
    public int[][] kClosest(int[][] points, int K) {
        // Max-heap with a custom comparator for negative distance
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> 
            (b[0] * b[0] + b[1] * b[1]) - (a[0] * a[0] + a[1] * a[1])
        );
        
        for (int[] point : points) {
            maxHeap.add(point);
            
            // If heap size exceeds K, remove the point with the maximum distance
            if (maxHeap.size() > K) {
                maxHeap.poll();
            }
        }
        
        // Convert the result from heap to array
        int[][] result = new int[K][2];
        for (int i = 0; i < K; i++) {
            result[i] = maxHeap.poll();
        }
        
        return result;
    }
}
```

#### Complexity:
- **Time Complexity**: O(N log K), where N is the number of points and K is the number of closest points required. This is due to the priority queue operations.
- **Space Complexity**: O(K), for storing the `K` closest points in the heap.

---

### Optimal Approach: Quickselect

#### Intuition:
Quickselect is an optimization over quicksort that allows us to partition the array and only focus on the `K` closest elements. It can achieve an average time complexity better than sorting or using a heap.

#### Steps:
1. Implement a partitioning method to organize points relative to a pivot such that all points closer than the pivot appear before all points farther than the pivot.
2. Recursively adjust the partitioning based on the position of K relative to the pivot's final index until the partition yielding the closest K points is achieved.

#### Code:
```java
import java.util.Random;

public class Solution {
    public int[][] kClosest(int[][] points, int K) {
        quickSelect(points, 0, points.length - 1, K);
        return Arrays.copyOfRange(points, 0, K);
    }
    
    private void quickSelect(int[][] points, int left, int right, int K) {
        if (left >= right) return;
        
        // Random pivot selection for improved average performance
        Random rand = new Random();
        int pivotIdx = left + rand.nextInt(right - left);
        
        // Partition the array around the pivot
        int pivotFinalIdx = partition(points, left, right, pivotIdx);
        
        // Recurse into the left or right half based on pivot position
        if (K < pivotFinalIdx) {
            quickSelect(points, left, pivotFinalIdx - 1, K);
        } else if (K > pivotFinalIdx) {
            quickSelect(points, pivotFinalIdx + 1, right, K);
        }
    }
    
    private int partition(int[][] points, int left, int right, int pivotIdx) {
        int pivotDist = distance(points[pivotIdx]);
        swap(points, pivotIdx, right);
        
        int storeIdx = left;
        for (int i = left; i < right; i++) {
            if (distance(points[i]) < pivotDist) {
                swap(points, storeIdx, i);
                storeIdx++;
            }
        }
        
        swap(points, storeIdx, right);
        return storeIdx;
    }
    
    private int distance(int[] point) {
        return point[0] * point[0] + point[1] * point[1];
    }
    
    private void swap(int[][] points, int i, int j) {
        int[] temp = points[i];
        points[i] = points[j];
        points[j] = temp;
    }
}
```

#### Complexity:
- **Time Complexity**: O(N) on average, but O(N^2) in the worst case due to the recursive partitioning process.
- **Space Complexity**: O(1), as it is an in-place sorting algorithm.

By providing these three solutions, you can choose based on your time complexity requirements and understand the transition from basic to more efficient methods dealing with finding the K closest points to the origin.

