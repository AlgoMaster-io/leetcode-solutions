# [120. Triangle](https://leetcode.com/problems/triangle/)

## Solutions

1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)

---

### Recursive Approach

In this approach, we start from the top of the triangle and explore all paths recursively to find the path with the minimum sum. The base case is when we reach the bottom of the triangle.

**Intuition:**

- Start from the top element and for each position `(i, j)`, move to the next row by choosing either `(i+1, j)` or `(i+1, j+1)`.
- Recursively calculate the path sum for each choice and return the minimal path sum.
- This approach explores all possible paths, leading to a time complexity that is exponential.

```python
def minimumTotal(triangle):
    def helper(i, j):
        # base case: if it's the last row, return the element itself
        if i == len(triangle) - 1:
            return triangle[i][j]
        
        # recursive step: consider the two possible paths
        left_path = helper(i + 1, j)
        right_path = helper(i + 1, j + 1)
        
        # return the minimum total from this point
        return triangle[i][j] + min(left_path, right_path)
    
    return helper(0, 0)
```

- **Time Complexity:** \(O(2^n)\) where `n` is the number of rows.
- **Space Complexity:** \(O(n)\) for the recursive call stack.

---

### Memoization Approach

To optimize the recursive approach, we store intermediate results in a cache, which avoids redundant calculations.

**Intuition:**

- Use a dictionary to store computed minimum paths from each position.
- Before calculating the minimum path for a position, check if it is already computed.

```python
def minimumTotal(triangle):
    def helper(i, j, memo):
        # if value is already calculated
        if (i, j) in memo:
            return memo[(i, j)]
        
        # base case
        if i == len(triangle) - 1:
            return triangle[i][j]
        
        # calculate the minimum path sum using recursion and save to memo
        left_path = helper(i + 1, j, memo)
        right_path = helper(i + 1, j + 1, memo)
        memo[(i, j)] = triangle[i][j] + min(left_path, right_path)
        
        return memo[(i, j)]
    
    return helper(0, 0, {})

```

- **Time Complexity:** \(O(n^2)\) where `n` is the number of rows.
- **Space Complexity:** \(O(n^2)\) for storing results.

---

### Dynamic Programming Approach

Dynamic programming uses an iterative bottom-up approach to solve this problem.

**Intuition:**

- Instead of starting from the top, we start filling up values from the bottom row to the top, storing computed path sums.
- For each cell, compute the minimum path sum by considering its two possible children below it. This is done in place.

```python
def minimumTotal(triangle):
    # Start from the second last row and move upwards
    for i in range(len(triangle) - 2, -1, -1):
        for j in range(len(triangle[i])):
            # update the current cell with the minimum path sum including children
            triangle[i][j] += min(triangle[i+1][j], triangle[i+1][j+1])
    
    # the top element now contains the minimum path sum
    return triangle[0][0]
```

- **Time Complexity:** \(O(n^2)\)
- **Space Complexity:** \(O(1)\), since computations are done in place, modifying the input triangle.

---

