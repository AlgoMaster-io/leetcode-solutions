# [Leetcode 36: Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approaches
1. [Naive Approach using Check Sets](#naive-approach-using-check-sets)
2. [Optimized Approach using Hash Sets](#optimized-approach-using-hash-sets)

### Naive Approach using Check Sets

#### Intuition
The main idea is to use three separate sets to track the values we see in each row, column, and sub-grid (commonly referred to as a box). For a board to be valid:
- Each row must have all unique numbers.
- Each column must have all unique numbers.
- Each of the 3x3 sub-boxes must contain unique numbers.

We'll iterate over each cell in the board. For each number (except '.'), check if it's been encountered before in the current row, column, or sub-box.

If we find a duplicate in any row, column, or box, the board is invalid.

#### Implementation

```python
def isValidSudoku(board):
    # Initialize sets to track seen numbers for rows, columns and boxes
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [set() for _ in range(9)]

    # Traverse each cell in the 9x9 board
    for r in range(9):
        for c in range(9):
            num = board[r][c]
            if num == '.':
                continue  # Skip empty cells

            # Calculate box index
            box_index = (r // 3) * 3 + (c // 3)

            # If num is seen before in the same row, column, or box, it's invalid
            if num in rows[r] or num in cols[c] or num in boxes[box_index]:
                return False

            # Mark num as seen in the current row, column, and box
            rows[r].add(num)
            cols[c].add(num)
            boxes[box_index].add(num)

    # If no duplicates found, the board is valid
    return True
```

#### Time and Space Complexity
- **Time Complexity**: O(1) because the board has a fixed size of 81 cells (9 rows x 9 columns).
- **Space Complexity**: O(1) since we use fixed-size additional data structures (sets) that only depend on the number of rows, columns, and boxes which are constant.

### Optimized Approach using Hash Sets

#### Intuition
The optimized approach is similar to the naive approach but combines the checks into a single set to slightly simplify and streamline the operations. We'll use a single hash set to store uniquely identified composite keys that represent each occupied cell's position in a row, column, and box. This method reduces overhead by combining three checks into a single operation.

A composite key can be in the format of `f"row-{r}-{num}"`, `f"col-{c}-{num}"`, or `f"box-{box_index}-{num}"`, allowing us to maintain uniqueness across rows, columns, and boxes with one HashSet.

#### Implementation

```python
def isValidSudokuOptimized(board):
    seen = set()  # A set to track the unique composite keys

    # Iterate through each cell of the board
    for r in range(9):
        for c in range(9):
            num = board[r][c]
            if num == '.':
                continue  # Ignore empty cells

            # Create composite keys for current row, column, and box
            row_key = f"row-{r}-{num}"
            col_key = f"col-{c}-{num}"
            box_key = f"box-{(r // 3) * 3 + (c // 3)}-{num}"

            # Check if any key is already in the seen set
            if row_key in seen or col_key in seen or box_key in seen:
                return False

            # Add the composite keys to the set
            seen.add(row_key)
            seen.add(col_key)
            seen.add(box_key)

    return True
```

#### Time and Space Complexity
- **Time Complexity**: O(1) because the operations are constrained to the fixed number of cells in the board.
- **Space Complexity**: O(1) as the maximum size of the set depends only on the 81 cells and three checks for each cell, making it constant.


