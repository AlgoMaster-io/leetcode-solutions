[Leetcode Problem 378: Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

## Solutions

- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Min-Heap](#approach-2-min-heap)
- [Approach 3: Binary Search](#approach-3-binary-search)

### Approach 1: Brute Force

**Intuition**:  
The simplest way to find the k-th smallest element in a matrix is to flatten the matrix into a single list of elements, sort this list, and then index the k-th smallest element directly. This is straightforward but not efficient, especially for larger matrices.

1. Convert the entire matrix into a single vector.
2. Sort the vector.
3. Retrieve the k-th element (1-indexed, which corresponds to the (k-1) index in 0-indexed system).

```cpp
#include <vector>
#include <algorithm>

int kthSmallest(std::vector<std::vector<int>>& matrix, int k) {
    std::vector<int> nums;
    
    // Flatten the matrix into a vector
    for (const auto& row : matrix) {
        for (int value : row) {
            nums.push_back(value);
        }
    }
    
    // Sort the vector
    std::sort(nums.begin(), nums.end());
    
    // Return the k-th smallest element
    return nums[k - 1];
}
```
**Time Complexity**: O(N^2 log N), where N is the number of rows (or columns, since it's an N x N matrix) because sorting is the most costly operation here.  
**Space Complexity**: O(N^2) for the additional space used to store the flattened matrix.

### Approach 2: Min-Heap

**Intuition**:  
The matrix is already sorted in a row-wise and column-wise manner, which allows us to use a min-heap to efficiently retrieve the k smallest elements. By using a priority queue (min-heap), we can always extract the smallest current element and then push the next element from the same row. 

1. Maintain a min-heap of size k.
2. Push the first element of each row into the heap.
3. Pop the smallest element from the heap, and push the next element from the same row.
4. Repeat until you have removed elements k times.

```cpp
#include <vector>
#include <queue>

struct Element {
    int value, row, col;
    bool operator>(const Element& other) const {
        return value > other.value;
    }
};

int kthSmallest(std::vector<std::vector<int>>& matrix, int k) {
    int n = matrix.size();
    std::priority_queue<Element, std::vector<Element>, std::greater<Element>> minHeap;

    // Initialize the min-heap with the first element of each row
    for (int row = 0; row < n; ++row) {
        minHeap.push({matrix[row][0], row, 0});
    }

    // Remove elements from the heap k-1 times
    for (int i = 0; i < k - 1; ++i) {
        Element current = minHeap.top();
        minHeap.pop();
        
        if (current.col < n - 1) { // Push the next element in the same row
            minHeap.push({matrix[current.row][current.col + 1], current.row, current.col + 1});
        }
    }

    // The k-th smallest element
    return minHeap.top().value;
}
```

**Time Complexity**: O(k log N) - where N is the matrix dimension, as we insert and extract k elements from the min-heap.  
**Space Complexity**: O(N) for the heap.

### Approach 3: Binary Search

**Intuition**:  
Instead of searching through the array element by element, we leverage the matrix's inherent sorted properties. We perform a binary search on the range of numbers in the matrix. For each middle candidate, utilize a "count less than" function that determines how many numbers in the matrix are less than or equal to this middle value.

1. Utilize binary search on the minimum and maximum values in the matrix.
2. Count elements less than or equal to the middle value during each iteration.
3. Adjust the search range based on this count in comparison to k.

```cpp
#include <vector>

int countLessEqual(const std::vector<std::vector<int>>& matrix, int mid, int& smaller, int& larger) {
    int count = 0;
    int n = matrix.size();
    int row = n - 1, col = 0;
    
    while (row >= 0 && col < n) {
        if (matrix[row][col] <= mid) {
            smaller = std::max(smaller, matrix[row][col]);
            count += row + 1;
            col++;
        } else {
            larger = std::min(larger, matrix[row][col]);
            row--;
        }
    }
    return count;
}

int kthSmallest(std::vector<std::vector<int>>& matrix, int k) {
    int n = matrix.size();
    int left = matrix[0][0], right = matrix[n-1][n-1];
    int result = left;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        int smaller = left, larger = right;
        
        int count = countLessEqual(matrix, mid, smaller, larger);
        
        if (count == k) {
            return smaller;
        }
        
        if (count < k) {
            left = larger;
        } else {
            right = smaller;
        }
    }
    return result;
}
```

**Time Complexity**: O(N log(max-min)) - N for counting and log(max-min) for the binary search range, where max-min is the range of numbers in the matrix.  
**Space Complexity**: O(1), as we only keep simple integer variables.

By adopting the binary search method, we effectively leverage the matrix's sorted characteristics, providing a highly efficient solution.

