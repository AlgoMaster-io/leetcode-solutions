You can find the problem [here](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/).

## Approaches
- [Brute Force](#brute-force)
- [Min-Heap](#min-heap)
- [Binary Search](#binary-search)

### Brute Force

Intuition:
- This approach involves flattening the given matrix into a single list, sorting the list, and then retrieving the k-th smallest element.
- Since the matrix is n x n and already sorted in each row and each column, converting it into a single sorted list would allow easy access to the k-th smallest element.

Steps:
1. Flatten the matrix into a one-dimensional list.
2. Sort the list.
3. Return the element at the (k-1)th index of the sorted list.

```csharp
public int KthSmallest_BruteForce(int[][] matrix, int k) {
    List<int> flattenedList = new List<int>();

    // Flatten the matrix
    foreach (var row in matrix) {
        flattenedList.AddRange(row);
    }

    // Sort the list
    flattenedList.Sort();

    // Get the k-th smallest element
    return flattenedList[k - 1];
}
```
**Time Complexity**: O(n^2 log n), where n is the size of the matrix (n^2 elements in total).
**Space Complexity**: O(n^2) due to the space used by the flattened list.

### Min-Heap (Priority Queue)

Intuition:
- We utilize a min-heap (priority queue) to maintain the order of elements as we extract the smallest one by one until reaching the k-th element.
- We put all elements into the min-heap and then remove the smallest element (heap root) k times.

Steps:
1. Insert all elements of the matrix into a min-heap.
2. Extract the minimum element from the heap k times.
3. The k-th extracted element is our answer.

```csharp
public int KthSmallest_MinHeap(int[][] matrix, int k) {
    int n = matrix.Length;
    PriorityQueue<int, int> minHeap = new PriorityQueue<int, int>();

    // Insert all elements into the min-heap
    for (int r = 0; r < n; r++) {
        for (int c = 0; c < n; c++) {
            minHeap.Enqueue(matrix[r][c], matrix[r][c]);
        }
    }

    // Extract min k times
    int element = 0;
    for (int i = 0; i < k; i++) {
        element = minHeap.Dequeue();
    }

    return element;
}
```
**Time Complexity**: O(n^2 log n), inserting each element takes log(n^2).
**Space Complexity**: O(n^2) for the heap storing all elements.

### Binary Search

Intuition:
- Utilize binary search on range rather than positions. We know the matrix is sorted in rows and columns, which allows us to narrow down where the k-th smallest element might be by using the matrix properties to count elements less than or equal to a mid-value.
  
Steps:
1. Define the smallest and largest elements: `low` is the first element of the matrix, and `high` is the last.
2. Perform binary search: calculate `mid`, and determine the count of elements less than or equal to `mid`.
3. If the count is less than `k`, adjust `low` to `mid+1`; otherwise, set `high` to `mid`.
4. Continue until `low` equals `high`, at which point we've found our value.

```csharp
public int KthSmallest_BinarySearch(int[][] matrix, int k) {
    int n = matrix.Length;
    int low = matrix[0][0];
    int high = matrix[n - 1][n - 1];

    while (low < high) {
        int mid = low + (high - low) / 2;
        int count = CountLessEqual(matrix, mid);

        if (count < k) {
            low = mid + 1;
        } else {
            high = mid;
        }
    }

    return low;
}

private int CountLessEqual(int[][] matrix, int value) {
    int n = matrix.Length;
    int row = n - 1;
    int col = 0;
    int count = 0;

    while (row >= 0 && col < n) {
        if (matrix[row][col] <= value) {
            // Count all elements in this column
            count += row + 1;
            col++;
        } else {
            // Move upwards
            row--;
        }
    }

    return count;
}
```
**Time Complexity**: O(n log(max-min)), where `max` and `min` are the maximum and minimum elements in the matrix.
**Space Complexity**: O(1), constant space.

