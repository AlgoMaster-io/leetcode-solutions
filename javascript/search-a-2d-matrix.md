# [Leetcode 74: Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

### Solutions:
- [Approach 1: Linear Search](#approach-1-linear-search)
- [Approach 2: Binary Search on Rows and Inner Row](#approach-2-binary-search-on-rows-and-inner-row)
- [Approach 3: Binary Search Treating the Matrix as a Flat Array](#approach-3-binary-search-treating-the-matrix-as-a-flat-array)

## Approach 1: Linear Search

### Intuition:
A simple way to solve the problem is by performing a linear search. We flatten the 2D matrix into a 1D list by iterating through each row and then each column in the row, checking if the target value exists.

### Steps:
1. Traverse through each row and within each row, traverse through each element of that row.
2. Compare each element with the target.
3. If found, return `true`; otherwise, continue until the end and return `false`.

### Complexity:
- **Time Complexity:** \(O(m \times n)\), where \(m\) is the number of rows and \(n\) is the number of columns.
- **Space Complexity:** \(O(1)\), as no additional space is utilized.

```javascript
var searchMatrix = function(matrix, target) {
    // Iterate through each row
    for (let i = 0; i < matrix.length; i++) {
        // Iterate through each element in the row
        for (let j = 0; j < matrix[i].length; j++) {
            // Check if current element is the target
            if (matrix[i][j] === target) {
                return true;
            }
        }
    }
    return false; // Return false if the target is not found
};
```

## Approach 2: Binary Search on Rows and Inner Row

### Intuition:
Since each row of the matrix is sorted, we can initially apply binary search across the first and last element of each row to identify the possible row which might contain the target, and then do another binary search within that row.

### Steps:
1. Perform a binary search to find the row which might contain the target.
2. If we identify a possible row, perform another binary search in that row.
3. If the target is present in the identified row during the inner binary search, return `true`; otherwise, return `false`.

### Complexity:
- **Time Complexity:** \(O(\log m + \log n)\) using two binary searches, where \(m\) is the number of rows and \(n\) is the number of columns.
- **Space Complexity:** \(O(1)\) as no additional structures are used.

```javascript
var searchMatrix = function(matrix, target) {
    if (!matrix.length || !matrix[0].length) return false;

    // Binary search to find the row
    let top = 0, bottom = matrix.length - 1;
    while (top <= bottom) {
        let rowMid = Math.floor((top + bottom) / 2);
        if (target < matrix[rowMid][0]) {
            bottom = rowMid - 1;
        } else if (target > matrix[rowMid][matrix[rowMid].length - 1]) {
            top = rowMid + 1;
        } else {
            break;
        }
    }

    if (top > bottom) return false;

    const row = Math.floor((top + bottom) / 2);

    // Binary search within the row
    let left = 0, right = matrix[row].length - 1;
    while (left <= right) {
        let colMid = Math.floor((left + right) / 2);
        if (matrix[row][colMid] === target) {
            return true;
        } else if (matrix[row][colMid] < target) {
            left = colMid + 1;
        } else {
            right = colMid - 1;
        }
    }
    return false;
};
```

## Approach 3: Binary Search Treating the Matrix as a Flat Array

### Intuition:
Consider the entire matrix as a single sorted array and perform a binary search. The index mapping from this conceptual array to the matrix can be achieved using row and column calculations.

### Steps:
1. Compute the flattened array's 'mid-point' and then calculate its corresponding row and column.
2. Perform regular binary search comparisons and recalculate boundaries based on conditions.
3. Return `true` if the target matches any mid-point value, else `false` after the loop.

### Complexity:
- **Time Complexity:** \(O(\log(m \times n))\)
- **Space Complexity:** \(O(1)\)

```javascript
var searchMatrix = function(matrix, target) {
    if (!matrix.length || !matrix[0].length) return false;
    
    let rows = matrix.length;
    let cols = matrix[0].length;
    let left = 0, right = rows * cols - 1;

    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        // Calculate mid element's row and column
        let midValue = matrix[Math.floor(mid / cols)][mid % cols];

        if (midValue === target) {
            return true;
        } else if (midValue < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return false;
};
```

These three approaches gradually improve in efficiency, with the final method harnessing the structure of the matrix most effectively for binary search.

