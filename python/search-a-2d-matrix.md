# [Leetcode 74: Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

## Approaches
- [Brute Force](#brute-force)
- [Binary Search on Each Row](#binary-search-on-each-row)
- [Optimal: Single Binary Search](#optimal-single-binary-search)

## Brute Force

### Intuition
The most straightforward approach to search in a 2D matrix is to iterate over every element in the matrix and check if it's equal to the target. Though simple, this method is inefficient for large matrices.

### Approach
- Traverse each row and column of the matrix.
- Compare each element with the target.
- If an element matches, return `True`.
- If no match is found after checking all elements, return `False`.

### Code

```python
def searchMatrix(matrix, target):
    # Traverse through each row
    for row in matrix:
        # Traverse through each element in the row
        for element in row:
            # If current element equals the target, return True
            if element == target:
                return True
    # Return False if the target is not found
    return False
```

### Complexity
- **Time Complexity:** O(m * n), where m is the number of rows and n is the number of columns in the matrix.
- **Space Complexity:** O(1), as no additional space is used.

## Binary Search on Each Row

### Intuition
Instead of checking each element, utilize the fact that each row of the matrix is sorted. By applying binary search on each row, we can reduce the average search time.

### Approach
- Iterate over each row.
- Apply binary search on the row.
- If the target is found in any row, return `True`.
- If no such row exists where the target is found, return `False`.

### Code

```python
def searchMatrix(matrix, target):
    # Function to perform binary search on a given row
    def binarySearch(row, target):
        left, right = 0, len(row) - 1
        while left <= right:
            mid = (left + right) // 2
            # Check if the mid element is the target
            if row[mid] == target:
                return True
            elif row[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return False

    # Apply binary search on each row
    for row in matrix:
        if binarySearch(row, target):
            return True
    return False
```

### Complexity
- **Time Complexity:** O(m * log n), as we perform binary search on each of the m rows.
- **Space Complexity:** O(1), constant space usage.

## Optimal: Single Binary Search

### Intuition
The matrix can be considered as a single sorted list since the rows are sorted and the first element of each row is greater than the last element of the previous row. We can use a single binary search treating the matrix as a flat array.

### Approach
1. Consider the matrix as a single sorted list of m*n elements.
2. Use binary search. Compute the middle index and translate it to row and column indices using:
   - Row index: `mid // n`
   - Column index: `mid % n`
3. Adjust the search space based on comparison of the middle element with the target.
4. Return `True` if found, otherwise `False` after the loop. 

### Code

```python
def searchMatrix(matrix, target):
    if not matrix or not matrix[0]:
        return False

    m, n = len(matrix), len(matrix[0])
    left, right = 0, m * n - 1

    # Perform binary search on the imaginary 1D list
    while left <= right:
        mid = (left + right) // 2
        row, col = divmod(mid, n)  # Determine the row and col in 2D matrix
        mid_element = matrix[row][col]
        
        # Compare mid element with the target
        if mid_element == target:
            return True
        elif mid_element < target:
            left = mid + 1
        else:
            right = mid - 1

    return False
```

### Complexity
- **Time Complexity:** O(log(m * n)), as we use binary search over a flattened representation of the matrix.
- **Space Complexity:** O(1), no additional space required beyond input storage.

