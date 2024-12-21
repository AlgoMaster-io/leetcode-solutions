# [Leetcode 74: Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Binary Search Approach](#binary-search-approach)

### Brute Force Approach

#### Intuition:
The simplest method to solve this problem is to iterate over each element of the 2D matrix and compare it with the target. If the target is found, return true. If the entire matrix is traversed without finding the target, return false. This approach directly follows the problem's statement without leveraging any optimization related to the sorted nature of the matrix rows or columns.

#### Code:
```typescript
function searchMatrixBruteForce(matrix: number[][], target: number): boolean {
    // Iterate over each row
    for (let row of matrix) {
        // Iterate over each element in the row
        for (let element of row) {
            // If the element matches the target, return true
            if (element === target) {
                return true;
            }
        }
    }
    // If the element isn't found in the entire matrix, return false
    return false;
}
```

#### Time Complexity:
- O(m * n), where `m` is the number of rows and `n` is the number of columns in the matrix.
- We potentially check each element once.

#### Space Complexity:
- O(1), no additional space is used other than variables.

### Binary Search Approach

#### Intuition:
Since each row of the matrix is sorted and each first element of a row is greater than the last element of the previous row, we can treat the matrix as a single sorted array. By doing this, we can effectively perform a binary search: calculate a middle index, convert it to 2D indices, and compare the middle element with the target. Adjust the search range based on the comparison.

#### Code:
```typescript
function searchMatrixBinarySearch(matrix: number[][], target: number): boolean {
    // Define the number of rows and columns
    const m = matrix.length;
    const n = matrix[0].length;

    // Set initial pointers for the binary search
    let left = 0;
    let right = m * n - 1;

    // Conduct binary search
    while (left <= right) {
        // Determine the mid index
        const mid = Math.floor((left + right) / 2);
        // Calculate the row and column from the mid index
        const midValue = matrix[Math.floor(mid / n)][mid % n];

        // Check if the midValue is equal to the target
        if (midValue === target) {
            return true;
        }
        // Adjust the right pointer if midValue is greater than target
        else if (midValue > target) {
            right = mid - 1;
        }
        // Adjust the left pointer if midValue is less than target
        else {
            left = mid + 1;
        }
    }

    // If not found, return false
    return false;
}
```

#### Time Complexity:
- O(log(m * n)), where `m` is the number of rows and `n` is the number of columns.
- This is because we are effectively performing a binary search on a 1D array.

#### Space Complexity:
- O(1), no additional space is used apart from a few pointers and temporary variables. 

By utilizing the sorted property of the matrix effectively, the binary search method provides a significant performance benefit over the brute force approach, especially for larger matrices.

