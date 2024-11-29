# [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

## Approach 1: Using HashSets for Rows, Columns, and Sub-boxes

### Solution
python
```python
# Time Complexity: O(9^2) = O(81)
# Space Complexity: O(9 * 9 * 3) = O(243) ≈ O(1) since the board size is constant
class Solution:
    def isValidSudoku(self, board):
        seen = set()

        # Iterate through the board
        for i in range(9):
            for j in range(9):
                num = board[i][j]

                # Ignore empty cells
                if num != '.':
                    # Construct unique keys for rows, columns, and boxes
                    rowKey = f"row{i}{num}"
                    colKey = f"col{j}{num}"
                    boxKey = f"box{i//3}{j//3}{num}"

                    # Check if the keys already exist
                    if rowKey in seen or colKey in seen or boxKey in seen:
                        return False
                    
                    seen.add(rowKey)
                    seen.add(colKey)
                    seen.add(boxKey)

        return True
```

## Approach 2: Using Arrays for Optimization

### Solution
python
```python
# Time Complexity: O(81) = O(1) for a fixed 9x9 board
# Space Complexity: O(81) = O(1)
class Solution:
    def isValidSudoku(self, board):
        rows = [[False] * 9 for _ in range(9)]
        cols = [[False] * 9 for _ in range(9)]
        boxes = [[False] * 9 for _ in range(9)]

        # Iterate through the board
        for i in range(9):
            for j in range(9):
                if board[i][j] != '.':
                    num = int(board[i][j]) - 1  # Map '1'-'9' to 0-8
                    boxIndex = (i // 3) * 3 + (j // 3)

                    # Check rows, columns, and boxes
                    if rows[i][num] or cols[j][num] or boxes[boxIndex][num]:
                        return False

                    # Mark the number as seen
                    rows[i][num] = True
                    cols[j][num] = True
                    boxes[boxIndex][num] = True

        return True
```

