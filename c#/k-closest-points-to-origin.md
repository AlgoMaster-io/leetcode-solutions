# [Leetcode 973: K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)

## Approaches:
1. [Brute Force with Sorting](#brute-force-with-sorting)
2. [Heap/Priority Queue Approach](#heap-priority-queue-approach)
3. [Quickselect Approach](#quickselect-approach)

### Brute Force with Sorting

**Intuition:**

The simplest approach to find the K closest points to the origin (0, 0) is to sort all the points according to their distance from the origin. Once sorted, the first K points in the sorted order will naturally be the closest.

Steps:
1. Calculate the square of the Euclidean distance for each point from the origin to avoid floating point operations.
2. Store these distances along with their respective points.
3. Sort the list of points based on their computed distances.
4. Extract the first K points from this sorted list.

**C# Code:**

```csharp
class Solution {
    public int[][] KClosest(int[][] points, int K) {
        // Calculate distance square of each point from the origin
        Array.Sort(points, (a, b) => ComparePoints(a, b));
        
        // Retrieve the first K points from the sorted points
        int[][] result = new int[K][];
        Array.Copy(points, 0, result, 0, K);
        
        return result;
    }
    
    private int ComparePoints(int[] a, int[] b) {
        // Compare based on the squared distance
        return (a[0] * a[0] + a[1] * a[1]).CompareTo(b[0] * b[0] + b[1] * b[1]);
    }
}
```
**Time Complexity:** O(N log N), where N is the number of points as we're sorting the points.

**Space Complexity:** O(1) for constant space apart from the input and output.

### Heap/Priority Queue Approach

**Intuition:**

Instead of sorting all points which can be costly, a more optimal method is to use a max-heap data structure to filter out the K closest points directly.

Steps:
1. Create a max-heap to keep track of the K closest points.
2. As we iterate over each point, compute its distance.
3. If the current point's distance is smaller than the max element in the heap, remove the max element and insert the current point.
4. At the end of iteration, the heap will contain the K closest points.

**C# Code:**

```csharp
class Solution {
    public int[][] KClosest(int[][] points, int K) {
        // Using max-heap to keep track of the k closest points
        var maxHeap = new SortedSet<(int Distance, int[] Point)>(Comparer<(int Distance, int[] Point)>.Create((a, b) => {
            int result = b.Distance.CompareTo(a.Distance);
            if (result == 0) result = b.Point[0].CompareTo(a.Point[0]); // Break ties
            return result;
        }));
        
        foreach (var point in points) {
            int dist = point[0] * point[0] + point[1] * point[1];
            maxHeap.Add((dist, point));
            if (maxHeap.Count > K) {
                maxHeap.Remove(maxHeap.Max);
            }
        }
        
        return maxHeap.Select(x => x.Point).ToArray();
    }
}
```

**Time Complexity:** O(N log K), `N` is the number of points. Inserting into the heap takes `O(log K)`.

**Space Complexity:** O(K) for storing the heap of K points.

### Quickselect Approach

**Intuition:**

The Quickselect algorithm is a selection algorithm to find the k-th smallest element in an unordered list. It is related to the QuickSort algorithm. We can adapt it to solve our problem in O(N) average time complexity.

Steps:
1. Implement the partitioning step similar to QuickSort.
2. Use the partition to systematically reduce the problem size until we have the desired K closest points.

**C# Code:**

```csharp
class Solution {
    public int[][] KClosest(int[][] points, int K) {
        // Using Quickselect to partition about K
        QuickSelect(points, 0, points.Length - 1, K);
        return points.Take(K).ToArray();
    }
    
    private void QuickSelect(int[][] points, int left, int right, int K) {
        if (left >= right) return;
        
        int pivotIndex = Partition(points, left, right);
        
        if (pivotIndex == K) {
            return;
        } else if (pivotIndex < K) {
            QuickSelect(points, pivotIndex + 1, right, K);
        } else {
            QuickSelect(points, left, pivotIndex - 1, K);
        }
    }
    
    private int Partition(int[][] points, int left, int right) {
        int[] pivot = points[right];
        int pivotDist = pivot[0] * pivot[0] + pivot[1] * pivot[1];
        int storeIndex = left;
        
        for (int i = left; i < right; i++) {
            if ((points[i][0] * points[i][0] + points[i][1] * points[i][1]) < pivotDist) {
                Swap(points, storeIndex, i);
                storeIndex++;
            }
        }
        
        Swap(points, storeIndex, right);
        return storeIndex;
    }
    
    private void Swap(int[][] points, int i, int j) {
        int[] temp = points[i];
        points[i] = points[j];
        points[j] = temp;
    }
}
```

**Time Complexity:** O(N) on average due to partitioning, where N is the number of points.

**Space Complexity:** O(log N) due to recursive stack space on average.

